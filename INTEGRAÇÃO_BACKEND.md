<!-- ============================================
INTEGRAÇÃO COM BACKEND - EXEMPLOS
Guia de integração do formulário de contato
com diferentes tecnologias backend
============================================ -->

# INTEGRAÇÃO COM BACKEND

## 📌 Overview

O formulário de contato foi desenvolvido com validação frontend, pronto para enviar dados para um backend. Este arquivo contém exemplos de integração com diferentes linguagens/frameworks.

---

## 🔗 JavaScript Frontend - Implementação Atual

### Arquivo: `script.js`

```javascript
// Seção 1: Função para enviar dados (descomentar e ajustar)
async function sendContactToBackend(data) {
    try {
        const response = await fetch('/api/contact', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'X-CSRF-Token': getCsrfToken() // Se necessário
            },
            body: JSON.stringify(data)
        });
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        return await response.json();
    } catch (error) {
        console.error('Erro ao enviar contato:', error);
        throw error;
    }
}

function getCsrfToken() {
    return document.querySelector('meta[name="csrf-token"]')?.content || '';
}
```

### Modificação no `initFormValidation()`:

```javascript
// Substituir a parte de simulação por:

// Preparar dados
const formData = {
    name: name,
    email: email,
    subject: subject,
    message: message,
    timestamp: new Date().toISOString()
};

// Enviar para backend
try {
    const result = await sendContactToBackend(formData);
    showFormAlert('✓ Mensagem enviada com sucesso!', 'success');
    contactForm.reset();
} catch (error) {
    showFormAlert('✗ Erro ao enviar mensagem. Tente novamente.', 'warning');
}
```

---

## 🐍 Backend - Python (Flask)

### Instalação

```bash
pip install flask flask-cors flask-mail python-dotenv
```

### Arquivo: `app.py`

```python
from flask import Flask, request, jsonify
from flask_cors import CORS
from flask_mail import Mail, Message
import os
from datetime import datetime
from dotenv import load_dotenv

load_dotenv()

app = Flask(__name__)
CORS(app)

# Configuração de Email
app.config['MAIL_SERVER'] = os.getenv('MAIL_SERVER', 'smtp.gmail.com')
app.config['MAIL_PORT'] = int(os.getenv('MAIL_PORT', 587))
app.config['MAIL_USE_TLS'] = os.getenv('MAIL_USE_TLS', True)
app.config['MAIL_USERNAME'] = os.getenv('MAIL_USERNAME')
app.config['MAIL_PASSWORD'] = os.getenv('MAIL_PASSWORD')
app.config['MAIL_DEFAULT_SENDER'] = os.getenv('MAIL_DEFAULT_SENDER', 'contato@visao-grupo.edu.br')

mail = Mail(app)

# ===== ROTA: Enviar Contato =====
@app.route('/api/contact', methods=['POST'])
def send_contact():
    """
    Recebe dados do formulário de contato
    Valida, salva em banco de dados (opcional)
    Envia email para administrador e confirmação para usuário
    """
    
    try:
        data = request.get_json()
        
        # Validação
        if not all(k in data for k in ['name', 'email', 'subject', 'message']):
            return jsonify({'error': 'Campos obrigatórios faltando'}), 400
        
        name = data.get('name', '').strip()
        email = data.get('email', '').strip()
        subject = data.get('subject', '').strip()
        message = data.get('message', '').strip()
        
        # Validação básica
        if len(name) < 3 or len(message) < 10:
            return jsonify({'error': 'Dados inválidos'}), 400
        
        # Salvar em banco de dados (exemplo com SQLite)
        # contact = Contact(name=name, email=email, subject=subject, message=message)
        # db.session.add(contact)
        # db.session.commit()
        
        # Email para administrador
        admin_msg = Message(
            subject=f'[VISÃO] Novo Contato: {subject}',
            recipients=['admin@visao-grupo.edu.br'],
            html=f'''
            <h2>Novo Contato Recebido</h2>
            <p><strong>Nome:</strong> {name}</p>
            <p><strong>Email:</strong> {email}</p>
            <p><strong>Assunto:</strong> {subject}</p>
            <p><strong>Mensagem:</strong></p>
            <p>{message}</p>
            <p><small>Recebido em: {datetime.now().strftime("%d/%m/%Y %H:%M:%S")}</small></p>
            '''
        )
        
        # Email de confirmação para usuário
        user_msg = Message(
            subject='[VISÃO] Recebemos sua mensagem',
            recipients=[email],
            html=f'''
            <h2>Obrigado por entrar em contato!</h2>
            <p>Olá {name},</p>
            <p>Recebemos sua mensagem e entraremos em contato em breve.</p>
            <hr>
            <p><strong>Seu Assunto:</strong> {subject}</p>
            <p>Equipe VISÃO</p>
            '''
        )
        
        # Enviar emails
        mail.send(admin_msg)
        mail.send(user_msg)
        
        return jsonify({
            'success': True,
            'message': 'Contato enviado com sucesso!',
            'id': data.get('email')  # ID da mensagem
        }), 200
        
    except Exception as e:
        print(f"Erro: {str(e)}")
        return jsonify({'error': f'Erro ao processar: {str(e)}'}), 500


# ===== ROTA: Saúde da API =====
@app.route('/api/health', methods=['GET'])
def health():
    return jsonify({'status': 'ok', 'timestamp': datetime.now().isoformat()}), 200


# ===== ROTA: Listar Contatos (Admin) =====
@app.route('/api/contacts', methods=['GET'])
def get_contacts():
    """
    Retorna todos os contatos (protegido por autenticação)
    """
    # Adicionar verificação de autenticação aqui
    
    # contacts = Contact.query.all()
    # return jsonify([c.to_dict() for c in contacts]), 200
    
    return jsonify({'error': 'Não implementado'}), 501


if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

### Arquivo: `.env`

```
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USE_TLS=True
MAIL_USERNAME=seu-email@gmail.com
MAIL_PASSWORD=sua-senha-de-app
MAIL_DEFAULT_SENDER=contato@visao-grupo.edu.br
```

---

## 🟢 Backend - Node.js (Express)

### Instalação

```bash
npm init -y
npm install express cors dotenv nodemailer express-validator
```

### Arquivo: `server.js`

```javascript
const express = require('express');
const cors = require('cors');
const nodemailer = require('nodemailer');
const { body, validationResult } = require('express-validator');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(cors());
app.use(express.json());

// Configuração de Email
const transporter = nodemailer.createTransport({
    service: 'gmail',
    auth: {
        user: process.env.MAIL_USERNAME,
        pass: process.env.MAIL_PASSWORD
    }
});

// ===== ROTA: Enviar Contato =====
app.post('/api/contact',
    // Validação
    body('name').notEmpty().isLength({ min: 3 }),
    body('email').isEmail(),
    body('subject').notEmpty(),
    body('message').isLength({ min: 10 }),
    async (req, res) => {
        // Verificar erros de validação
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
            return res.status(400).json({ errors: errors.array() });
        }
        
        try {
            const { name, email, subject, message } = req.body;
            
            // Email para administrador
            await transporter.sendMail({
                from: process.env.MAIL_DEFAULT_SENDER,
                to: 'admin@visao-grupo.edu.br',
                subject: `[VISÃO] Novo Contato: ${subject}`,
                html: `
                    <h2>Novo Contato Recebido</h2>
                    <p><strong>Nome:</strong> ${name}</p>
                    <p><strong>Email:</strong> ${email}</p>
                    <p><strong>Assunto:</strong> ${subject}</p>
                    <p><strong>Mensagem:</strong></p>
                    <p>${message}</p>
                    <p><small>Recebido em: ${new Date().toLocaleString('pt-BR')}</small></p>
                `
            });
            
            // Email de confirmação
            await transporter.sendMail({
                from: process.env.MAIL_DEFAULT_SENDER,
                to: email,
                subject: '[VISÃO] Recebemos sua mensagem',
                html: `
                    <h2>Obrigado por entrar em contato!</h2>
                    <p>Olá ${name},</p>
                    <p>Recebemos sua mensagem e entraremos em contato em breve.</p>
                    <hr>
                    <p><strong>Seu Assunto:</strong> ${subject}</p>
                    <p>Equipe VISÃO</p>
                `
            });
            
            res.json({
                success: true,
                message: 'Contato enviado com sucesso!'
            });
            
        } catch (error) {
            console.error('Erro:', error);
            res.status(500).json({ error: 'Erro ao processar contato' });
        }
    }
);

// ===== ROTA: Saúde =====
app.get('/api/health', (req, res) => {
    res.json({ status: 'ok', timestamp: new Date().toISOString() });
});

app.listen(PORT, () => {
    console.log(`Servidor VISÃO rodando na porta ${PORT}`);
});
```

### Arquivo: `.env`

```
MAIL_USERNAME=seu-email@gmail.com
MAIL_PASSWORD=sua-senha-de-app
MAIL_DEFAULT_SENDER=contato@visao-grupo.edu.br
PORT=5000
```

---

## 🐘 Backend - PHP

### Arquivo: `api/contact.php`

```php
<?php
// Permitir CORS
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: POST, GET, OPTIONS');
header('Access-Control-Allow-Headers: Content-Type');
header('Content-Type: application/json; charset=utf-8');

// Responder a OPTIONS
if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
    http_response_code(200);
    exit;
}

// Receber dados JSON
$data = json_decode(file_get_contents('php://input'), true);

// Validação
if (!isset($data['name'], $data['email'], $data['subject'], $data['message'])) {
    http_response_code(400);
    echo json_encode(['error' => 'Campos obrigatórios faltando']);
    exit;
}

$name = htmlspecialchars($data['name']);
$email = htmlspecialchars($data['email']);
$subject = htmlspecialchars($data['subject']);
$message = htmlspecialchars($data['message']);

// Validar email
if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
    http_response_code(400);
    echo json_encode(['error' => 'Email inválido']);
    exit;
}

// Validar tamanho mínimo
if (strlen($name) < 3 || strlen($message) < 10) {
    http_response_code(400);
    echo json_encode(['error' => 'Dados inválidos']);
    exit;
}

try {
    // Email para administrador
    $to_admin = 'admin@visao-grupo.edu.br';
    $subject_admin = "[VISÃO] Novo Contato: $subject";
    $message_admin = "
    <h2>Novo Contato Recebido</h2>
    <p><strong>Nome:</strong> $name</p>
    <p><strong>Email:</strong> $email</p>
    <p><strong>Assunto:</strong> $subject</p>
    <p><strong>Mensagem:</strong></p>
    <p>$message</p>
    <p><small>Recebido em: " . date('d/m/Y H:i:s') . "</small></p>
    ";
    
    // Headers
    $headers = "MIME-Version: 1.0\r\n";
    $headers .= "Content-type: text/html; charset=UTF-8\r\n";
    $headers .= "From: " . $email . "\r\n";
    
    // Enviar email para admin
    mail($to_admin, $subject_admin, $message_admin, $headers);
    
    // Email de confirmação para usuário
    $subject_user = '[VISÃO] Recebemos sua mensagem';
    $message_user = "
    <h2>Obrigado por entrar em contato!</h2>
    <p>Olá $name,</p>
    <p>Recebemos sua mensagem e entraremos em contato em breve.</p>
    <hr>
    <p><strong>Seu Assunto:</strong> $subject</p>
    <p>Equipe VISÃO</p>
    ";
    
    mail($email, $subject_user, $message_user, $headers);
    
    // Salvar em banco de dados (opcional)
    // saveContactToDatabase($name, $email, $subject, $message);
    
    http_response_code(200);
    echo json_encode([
        'success' => true,
        'message' => 'Contato enviado com sucesso!'
    ]);
    
} catch (Exception $e) {
    http_response_code(500);
    echo json_encode(['error' => 'Erro ao processar contato']);
}
?>
```

---

## 🗄️ Banco de Dados - Schema SQL

### SQLite / MySQL

```sql
CREATE TABLE contacts (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    subject VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    ip_address VARCHAR(45),
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    read_at TIMESTAMP NULL,
    responded BOOLEAN DEFAULT FALSE
);

CREATE TABLE contact_responses (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    contact_id INTEGER NOT NULL,
    response TEXT NOT NULL,
    sender_email VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (contact_id) REFERENCES contacts(id) ON DELETE CASCADE
);

CREATE INDEX idx_email ON contacts(email);
CREATE INDEX idx_created_at ON contacts(created_at);
```

---

## 🔐 Segurança

### Implementações Recomendadas

1. **CSRF Protection**
   ```html
   <meta name="csrf-token" content="{{ csrf_token }}">
   ```

2. **Rate Limiting**
   ```javascript
   // Node.js
   const rateLimit = require('express-rate-limit');
   const limiter = rateLimit({
       windowMs: 15 * 60 * 1000,
       max: 5 // 5 requisições por IP
   });
   app.post('/api/contact', limiter, (req, res) => { });
   ```

3. **Sanitização de Entrada**
   ```python
   # Python
   from bleach import clean
   message = clean(message, strip=True)
   ```

4. **HTTPS**
   - Use sempre em produção

5. **Autenticação**
   - Implemente JWT ou Sessions para endpoints admin

---

## 📧 Exemplo de Email HTML

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <style>
        body { font-family: Arial, sans-serif; background: #f8f9fa; }
        .container { max-width: 600px; margin: 0 auto; background: white; padding: 20px; }
        .header { background: linear-gradient(135deg, #003d7a, #0066cc); color: white; padding: 20px; text-align: center; }
        .content { padding: 20px; }
        .footer { background: #f8f9fa; padding: 10px; text-align: center; font-size: 12px; }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>VISÃO - Grupo de Pesquisa</h1>
        </div>
        <div class="content">
            <h2>Obrigado por entrar em contato!</h2>
            <p>Olá [NOME],</p>
            <p>Recebemos sua mensagem e entraremos em contato em breve.</p>
            <p><strong>Assunto:</strong> [SUBJECT]</p>
        </div>
        <div class="footer">
            <p>&copy; 2024 VISÃO - Todos os direitos reservados</p>
        </div>
    </div>
</body>
</html>
```

---

## 🧪 Testes

### cURL - Testar API

```bash
curl -X POST http://localhost:5000/api/contact \
  -H "Content-Type: application/json" \
  -d '{
    "name": "João Silva",
    "email": "joao@example.com",
    "subject": "Teste de Contato",
    "message": "Esta é uma mensagem de teste para verificar se o sistema está funcionando corretamente."
  }'
```

### Postman
1. Abra Postman
2. Crie nova requisição POST
3. URL: `http://localhost:5000/api/contact`
4. Body (JSON):
```json
{
    "name": "João Silva",
    "email": "joao@example.com",
    "subject": "Teste",
    "message": "Testando API de contato"
}
```

---

## 📊 Monitoramento

### Logs Recomendados

```python
import logging

logging.basicConfig(
    filename='contacts.log',
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

@app.route('/api/contact', methods=['POST'])
def send_contact():
    try:
        logging.info(f'Novo contato de {email}')
        # ...
    except Exception as e:
        logging.error(f'Erro: {str(e)}')
```

---

## 🎯 Checklist de Deploy

- [ ] Validação frontend funcionando
- [ ] Validação backend implementada
- [ ] Email sendo enviado
- [ ] Banco de dados configurado
- [ ] HTTPS ativado
- [ ] CORS configurado corretamente
- [ ] Rate limiting ativo
- [ ] Logs sendo registrados
- [ ] Backup automático
- [ ] Monitoramento ativo

---

## 📚 Referências

- [MDN - Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Flask Documentation](https://flask.palletsprojects.com)
- [Express.js Guide](https://expressjs.com)
- [PHP Mail Documentation](https://www.php.net/manual/en/function.mail.php)
- [Nodemailer](https://nodemailer.com)

---

**Desenvolvido para o Grupo de Pesquisa VISÃO** ✨
