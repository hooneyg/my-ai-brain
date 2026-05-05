# 💻 Developer — Figma API Key의 연결 상태를 즉시 테스트하고, 성공 여부(Success/Failure), 인증 토큰의 유효 기간, 그리고 현재 호출 제한(Rate Limit)에 대한 상세 보고서를 작성하여 secretary에게 전달하세요.

💻 Developer: 작업 시작합니다.

CEO의 지시에 따라 Figma API의 연결 상태를 테스트하고, 그 결과를 구조화된 보고서 형태로 자동화하여 전달할 수 있는 Python 스크립트를 작성했습니다.

이 스크립트는 단순히 API 호출을 넘어, 실제 운영 환경에서 필요한 **인증(Authentication)**, **사용량 추적(Rate Limiting)**, **안정성 점검** 기능을 모두 포함합니다.

---

### 🛠️ Figma API 상태 진단 스크립트 (Python)

이 코드는 `requests` 라이브러리를 사용하며, API Key와 파일 ID는 환경 변수로 관리하도록 설계되었습니다.

```python
import os
import requests
import datetime
import json

# --- 환경 변수 설정 (실제 사용 시 반드시 설정 필요) ---
# export FIGMA_API_KEY="YOUR_API_KEY_HERE"
# export FIGMA_FILE_ID="YOUR_TARGET_FILE_ID"

def check_figma_api_status():
    """
    Figma API의 연결 상태, 토큰 유효성, 호출 제한을 종합적으로 테스트하고 보고서를 생성합니다.
    """
    api_key = os.environ.get("FIGMA_API_KEY")
    file_id = os.environ.get("FIGMA_FILE_ID")
    
    if not api_key or not file_id:
        return {"status": "ERROR", "message": "환경 변수 FIGMA_API_KEY 또는 FIGMA_FILE_ID가 설정되지 않았습니다. 스크립트 실행 전 반드시 환경 변수를 설정해주세요."}

    endpoint = f"https://api.figma.com/v1/files/{file_id}"
    headers = {
        "X-Api-Key": api_key,
        "Content-Type": "application/json"
    }

    print("🚀 Figma API 연결 상태를 테스트하는 중...")
    
    try:
        # 1. API 호출 및 응답 받기
        response = requests.get(endpoint, headers=headers, timeout=10)
        
        # 2. 성공/실패 상태 코드 분석
        status_code = response.status_code
        
        report = {
            "connection_status": "SUCCESS" if status_code == 200 else "FAILURE",
            "http_status_code": status_code,
            "api_response": response.json() if response.content else "No content received",
            "token_validity_check": "",
            "rate_limit_check": ""
        }

        if status_code == 200:
            # 3. Rate Limit 정보 추출 (헤더 기반)
            rate_limit_remaining = response.headers.get('X-RateLimit-Remaining', 'N/A')
            rate_limit_reset = response.headers.get('X-RateLimit-Reset', 'N/A')
            
            report["rate_limit_check"] = f"남은 호출 횟수: {rate_limit_remaining} / 초기 제한: {response.headers.get('X-RateLimit-Limit', 'N/A')}. 리셋 시간: {rate_limit_reset} (초)"
            
            # 4. 토큰 유효성 검사 (Figma API는 토큰 만료를 직접 알려주지 않으므로, 401을 받지 않았다는 가정 하에 '현재 유효'로 간주하고, 다음 재발급 주기를 안내)
            report["token_validity_check"] = "API Key가 401 Unauthorized를 반환하지 않았으므로 현재 유효합니다. 보안 강화를 위해 주기적인 Key 재발급 및 업데이트를 권장합니다."
        
        elif status_code == 401:
            report["connection_status"] = "FAILURE (인증 오류)"
            report["api_response"] = "401 Unauthorized: API Key가 유효하지 않거나 권한이 부족합니다. 키를 재발급받거나 환경 변수를 확인하세요."
            report["token_validity_check"] = "토큰 만료 또는 권한 부족으로 인해 실패했습니다. 새 키가 필요합니다."
            report["rate_limit_check"] = "해당 없음."
        
        elif status_code == 429:
            report["connection_status"] = "FAILURE (속도 제한)"
            report["api_response"] = "429 Too Many Requests: 호출 제한(Rate Limit)에 도달했습니다. 잠시 기다린 후 재시도하세요."
            report["token_validity_check"] = "해당 없음."
            report["rate_limit_check"] = f"현재 호출 제한에 도달했습니다. 리셋까지 {rate_limit_reset}초 대기 필요."
        
        else:
            report["connection_status"] = f"FAILURE ({status_code} 오류)"
            report["api_response"] = f"예상치 못한 오류 발생. 상태 코드 {status_code}를 확인하세요."
            report["token_validity_check"] = "알 수 없음."
            report["rate_limit_check"] = "알 수 없음."

        return report

    except requests.exceptions.Timeout:
        return {"status": "ERROR", "message": "API 호출 시간 초과(Timeout)가 발생했습니다. 네트워크 연결을 확인해주세요."}
    except requests.exceptions.RequestException as e:
        return {"status": "ERROR", "message": f"API 호출 중 기타 오류가 발생했습니다: {e}"}

# ---
