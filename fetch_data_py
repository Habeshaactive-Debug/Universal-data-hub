import os
import json
import requests

# 1. FUNKTION FÜR DIE KI-UNTERSTÜTZUNG (GROQ API)
def ask_ai(prompt):
    api_key = os.getenv("GROQ_API_KEY")
    if not api_key:
        return {"error": "API Key fehlt"}
    url = "https://api.groq.com/openai/v1/chat/completions"
    headers = {"Authorization": f"Bearer {api_key}", "Content-Type": "application/json"}
    data = {
        "model": "llama3-8b-8192",
        "messages": [{"role": "user", "content": prompt + " Antworte NUR mit reinem JSON, kein Text drumherum!"}],
        "response_format": {"type": "json_object"}
    }
    try:
        res = requests.post(url, json=data, headers=headers).json()
        return json.loads(res['choices'][0]['message']['content'])
    except:
        return {"error": "KI-Fehler"}

# SÄULE A: KI-TRENDS & GITHUB-MODELLE
def get_ai_trends():
    prompt = "Generiere eine Liste der aktuell 3 trendigsten Open-Source KI-Modelle auf GitHub mit Name, Entwickler und Haupt-Einsatzzweck."
    return ask_ai(prompt)

# SÄULE B: FINANZ- & KRYPTO-DATEN (Echte Live-Preise von CoinGecko)
def get_crypto_data():
    try:
        res = requests.get("https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,solana&vs_currencies=eur").json()
        return res
    except:
        return {"error": "Krypto-API nicht erreichbar"}

# SÄULE C: REMOTE JOBS
def get_remote_jobs():
    prompt = "Generiere eine Liste von 3 aktuellen, hochbezahlten Remote-Python/AI-Entwickler-Jobs weltweit inklusive Firma, Gehaltsschätzung in USD und Link-Dummy."
    return ask_ai(prompt)

# ALLES ZUSAMMENFÜHREN & SPEICHERN
def main():
    database = {
        "meta": {"status": "online", "last_update": "2026-stündlich"},
        "ai_trends": get_ai_trends(),
        "crypto_markets": get_crypto_data(),
        "remote_jobs": get_remote_jobs()
    }
    
    with open("data.json", "w", encoding="utf-8") as f:
        json.dump(database, f, indent=4, ensure_ascii=False)
    print("Daten erfolgreich aktualisiert!")

if __name__ == "__main__":
    main()
