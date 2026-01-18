# Mellow Fellow: High-Velocity Inference Engine

Mellow Fellow is a high-performance logic processing backend engineered for ultra-low latency intelligence tasks. It serves as a dedicated gateway between the Groq LPU™ Inference Engine and custom client applications.

## Tech Stack
- **Engine**: Groq LPU™ Reference Architecture.
- **Model**: Meta Llama 3.1 (8B Instant).
- **Framework**: Flask (Python) with Gunicorn for production concurrency.
- **Deployment**: Optimized for containerized PaaS environments.

## Deployment Specifications (Render)

### 1. Configuration
- **Runtime**: Python 3.x
- **Build Command**: pip install -r requirements.txt
- **Start Command**: gunicorn app:app

### 2. Environment Security
The system requires a secure environment variable for operation:
- **Key**: GROQ_API_KEY
- **Value**: [Restricted API Key]

## System Architecture (app.py)

import os
from flask import Flask, request, jsonify
from flask_cors import CORS
from groq import Groq

app = Flask(__name__)
CORS(app)

client = Groq(api_key=os.environ.get("GROQ_API_KEY"))

@app.route('/process', methods=['POST'])
def process():
    data = request.json
    try:
        response = client.chat.completions.create(
            model="llama-3.1-8b-instant",
            messages=[{"role": "user", "content": data['payload']}]
        )
        return jsonify({"result": response.choices[0].message.content})
    except Exception as e:
        return jsonify({"error": "Internal System Error"}), 500

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

## Core Dependencies (requirements.txt)

flask
flask-cors
groq
gunicorn

## Lisence
This project is open-source and free to use, modify, or edit.
