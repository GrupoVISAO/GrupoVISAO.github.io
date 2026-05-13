# VISÃO - Veículos Inteligentes e Sistemas Autônomos Otimizados

## 🚀 Site Institucional Moderno e Responsivo

Um site institucional profissional, moderno e tecnológico para o grupo de pesquisa **VISÃO**, desenvolvido com HTML5, CSS3 e JavaScript puro.

---

## 📋 Características Principais

### Design e Visual
- ✨ **Tema Tecnológico e Futurista** - Design inspirado em laboratórios de IA e grupos de robótica
- 🎨 **Paleta de Cores Profissional** - Azul escuro, azul claro, verde tecnológico e cinza
- 📱 **100% Responsivo** - Desktop, tablet e celular
- ✅ **Glassmorphism Leve** - Efeitos modernos em seções estratégicas
- 🎭 **Animações Suaves** - Transições elegantes e interações fluidas
- 🔄 **Modo Escuro Pronto** - Compatibilidade com preferências do sistema

### Funcionalidades
- 📌 **Menu Sticky** - Navegação sempre acessível
- 📱 **Menu Mobile Hamburger** - Navegação otimizada para dispositivos móveis
- ⬆️ **Botão Voltar ao Topo** - Acesso rápido
- 🎬 **Animações On-Scroll** - Elementos animam ao entrar em vista
- 📝 **Validação de Formulário** - Validação de email e campos obrigatórios
- 🖼️ **Canvas Animations** - Fundo animado com partículas e circuitos
- 🎯 **Links Suavizados** - Smooth scroll para toda a página
- 💾 **Preparado para Backend** - Estrutura pronta para integração com servidor

---

## 🏗️ Estrutura do Projeto

```
grupo_pesquisa_visao/
├── index.html          # Estrutura HTML principal
├── style.css           # Estilos CSS3 moderno e responsivo
├── script.js           # Funcionalidades JavaScript puras
└── README.md           # Este arquivo
```

### Arquivos Principais

#### **index.html**
- Estrutura semântica HTML5
- Seções bem organizadas
- Meta tags para SEO
- Bootstrap 5 para layout responsivo
- Font Awesome para ícones

#### **style.css**
- **1000+ linhas** de CSS moderno
- Variáveis CSS para temas
- Animações suaves
- Efeitos Glassmorphism
- Media queries responsivas
- Sistema de grid flexível

#### **script.js**
- **400+ linhas** de JavaScript
- Sem dependências externas (exceto Bootstrap)
- Funções bem comentadas
- Estrutura modular e extensível
- Tratamento de erros

---

## 📄 Seções do Site

### 1. **HERO SECTION**
- Logo do grupo VISÃO
- Título e subtítulo impactantes
- Botões de ação (CTA)
- Fundo animado com canvas
- Indicador de scroll

### 2. **SOBRE O GRUPO**
- Descrição institucional
- Boxes de características principais
- Design com glassmorphism
- Animações on-scroll

### 3. **LINHAS DE PESQUISA**
- 3 cards de pesquisa principais:
  - Percepção e Navegação
  - Controle e Atuação
  - Propulsão e Mecanização
- Listagem de tecnologias por linha
- Hover effects

### 4. **ÁREAS TECNOLÓGICAS**
- 8 cards com ícones
- Inteligência Artificial
- Visão Computacional
- Robótica
- Sistemas Embarcados
- Controle
- IoT
- Navegação Autônoma
- Sensoriamento

### 5. **PROJETOS**
- 5 cards de projetos fictícios
- Imagens com gradientes
- Tags de tecnologia
- Botões "Saiba Mais"

### 6. **EQUIPE**
- Seção de coordenador
- Pesquisadores sênior
- Alunos de pós-graduação
- Parceiros e colaboradores
- Cards com placeholder de fotos

### 7. **PUBLICAÇÕES**
- Artigos em periódicos
- Trabalhos em congressos
- Dissertações e TCCs
- Design moderno de lista

### 8. **CONTATO**
- Formulário com validação
- Informações de contato
- Links de redes sociais
- Mapa placeholder
- Integração pronta para backend

### 9. **FOOTER**
- Logo VISÃO
- Links rápidos
- Redes sociais
- Copyright e políticas

---

## 🎨 Paleta de Cores

```css
--primary-color: #003d7a;       /* Azul Escuro */
--secondary-color: #0066cc;     /* Azul Claro */
--accent-color: #28a745;        /* Verde Tecnológico */
--light-bg: #f8f9fa;            /* Cinza Muito Claro */
```

---

## 📦 Dependências Externas

### CDN (Já inclusos no HTML)
- **Bootstrap 5** - Layout responsivo
- **Font Awesome 6** - Ícones profissionais

### Nativa
- HTML5
- CSS3
- JavaScript ES6

---

## 🚀 Como Usar

### 1. **Abrir o Site**
```bash
# Simplesmente abra o arquivo index.html em seu navegador
# Ou use um servidor local
python -m http.server 8000
# Acesse: http://localhost:8000
```

### 2. **Estrutura de Pastas Recomendada**
```
projeto/
├── index.html
├── style.css
├── script.js
├── assets/
│   ├── images/
│   ├── logos/
│   └── icons/
└── README.md
```

### 3. **Customização Básica**

#### Mudar Cores
Edite as variáveis CSS no início do `style.css`:
```css
:root {
    --primary-color: #003d7a;       /* Mude aqui */
    --secondary-color: #0066cc;     /* Mude aqui */
    --accent-color: #28a745;        /* Mude aqui */
}
```

#### Atualizar Logo
No `index.html`, atualize a tag `<a class="navbar-brand">`:
```html
<a class="navbar-brand" href="#home">
    <img src="seu-logo.png" alt="VISÃO" class="navbar-logo">
    VISÃO
</a>
```

#### Adicionar Conteúdo
Cada seção possui um ID (ex: `id="linhas"`), facilitando edições.

---

## 🔧 Funcionalidades JavaScript

### Canvas Animation
```javascript
initCanvasBackground();
```
Anima o fundo da hero section com partículas conectadas.

### Scroll Animations
```javascript
initScrollAnimations();
```
Elementos aparecem com animação ao entrar em vista.

### Form Validation
```javascript
initFormValidation();
```
Valida email, campos obrigatórios e tamanho mínimo.

### Smooth Scroll
```javascript
initSmoothScroll();
```
Links internos com scroll suave.

### Back to Top
```javascript
initBackToTopButton();
```
Botão flutuante para voltar ao topo.

---

## 📱 Responsividade

### Breakpoints
- **Desktop**: > 1200px
- **Tablets**: 768px - 1200px
- **Celulares**: < 768px
- **Pequenos**: < 480px

### Teste
```css
/* Desktop */
@media (max-width: 1200px) { }

/* Tablet */
@media (max-width: 768px) { }

/* Celular */
@media (max-width: 480px) { }
```

---

## 🔐 SEO e Acessibilidade

### Meta Tags
- Description
- Viewport
- Open Graph (pronto para adicionar)

### Acessibilidade
- Navegação por teclado
- Contraste de cores adequado
- Labels em formulários
- Hierarquia de headings apropriada

---

## 🎯 Próximos Passos - Expansão Futura

### Backend Integration
```javascript
// Descomentar em script.js para conectar com API
async function sendContactToBackend(data) {
    const response = await fetch('/api/contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(data)
    });
    return await response.json();
}
```

### Adicionar Funcionalidades

#### 1. **Galeria de Imagens**
```html
<div class="gallery">
    <img src="imagem.jpg" data-src="full-size.jpg" alt="Projeto">
</div>
```

#### 2. **Blog/Notícias**
```html
<section id="blog" class="section-blog">
    <!-- Cards de artigos -->
</section>
```

#### 3. **Newsletter**
```html
<form class="newsletter-form">
    <input type="email" placeholder="Seu email...">
    <button>Inscrever</button>
</form>
```

#### 4. **Chat com IA**
```javascript
// Integrar chat widget para contato automático
```

#### 5. **Statisticas/Analytics**
```javascript
// Google Analytics ou Plausible
```

---

## 💡 Dicas de Desenvolvimento

### Adicionar Nova Seção
1. Crie um `<section>` no HTML com ID único
2. Escreva CSS correspondente em `style.css`
3. Adicione link no navbar e footer
4. Teste responsividade

### Otimizar Imagens
```html
<!-- Lazy Loading -->
<img src="placeholder.jpg" data-src="full-image.jpg" alt="Descrição">
```

### Adicionar Animação Customizada
```css
@keyframes minha-animacao {
    from { transform: scale(0); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
}

.elemento {
    animation: minha-animacao 0.5s ease-out;
}
```

---

## 🐛 Troubleshooting

### Canvas não aparece
- Verifique se JavaScript está habilitado
- Verifique console para erros

### Formulário não funciona
- Verifique email válido
- Verifique campos obrigatórios
- Veja console para mensagens de erro

### Menu mobile não funciona
- Certifique-se Bootstrap JS está carregado
- Verifique se html tem `id="navbarMenu"`

### Animações não funcionam
- Verifique se CSS está sendo carregado
- Verifique se elementos têm classes corretas

---

## 📈 Performance

### Otimizações Implementadas
- CSS minificável (~50KB)
- JavaScript sem minificação obrigatória (~15KB)
- Canvas em vez de GIFs/imagens
- Lazy loading pronto para imagens
- Font Awesome otimizado

### Sugestões
- Minificar CSS/JS em produção
- Usar GZIP compression
- CDN para arquivos estáticos
- Cache de navegador

---

## 🤝 Contribuindo

Para expandir o site:
1. Crie comentários em suas mudanças
2. Mantenha estrutura modular
3. Teste responsividade
4. Verifique compatibilidade de navegadores

---

## 📝 Licença

Este projeto é de uso exclusivo do Grupo de Pesquisa VISÃO.

---

## 📞 Suporte e Contato

**Email**: contato@visao-grupo.edu.br  
**Telefone**: (+55) 11 3333-4444  

---

## 🎓 Tecnologias Utilizadas

- ![HTML5](https://img.shields.io/badge/HTML5-E34C26?style=flat&logo=html5&logoColor=white)
- ![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
- ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
- ![Bootstrap5](https://img.shields.io/badge/Bootstrap5-7952B3?style=flat&logo=bootstrap&logoColor=white)
- ![FontAwesome](https://img.shields.io/badge/FontAwesome-228AE6?style=flat&logo=fontawesome&logoColor=white)

---

## ✨ Características Especiais

✅ Sem frameworks pesados  
✅ Carregamento rápido  
✅ Design responsivo  
✅ Animações suaves  
✅ Acessível  
✅ SEO optimizado  
✅ Pronto para backend  
✅ Bem comentado  
✅ Estrutura modular  
✅ Fácil de manter  

---

**Desenvolvido com ❤️ para o Grupo de Pesquisa VISÃO**

Última atualização: 2024
