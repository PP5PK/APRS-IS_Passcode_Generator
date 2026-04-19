# 📡 Gerador de Senhas APRS-IS

Ferramenta web para geração de passcodes de acesso à rede **APRS-IS** (*Automatic Packet Reporting System – Internet Service*), baseada no indicativo de chamada da estação.

Desenvolvida com estética de terminal retrô, sem dependências externas e totalmente client-side — o cálculo é feito diretamente no navegador, sem envio de dados a servidores.

---

## 🌐 Acesso

> **[aprs.dvbr.net](https://aprs.dvbr.net)**

---

## ✨ Funcionalidades

- **Cálculo automático com debounce** — o passcode é gerado automaticamente 2 segundos após a digitação parar (mínimo de 3 caracteres)
- **Cálculo imediato via Enter** — pressionar `Enter` dispara o cálculo sem esperar o debounce
- **Cópia automática para a área de transferência** — ao calcular, o passcode é copiado automaticamente
- **Botão de cópia manual** — permite copiar novamente a qualquer momento
- **Validação e sanitização em tempo real** — apenas caracteres alfanuméricos são aceitos; a entrada é convertida automaticamente para maiúsculas
- **Contador de caracteres** — com indicação visual de aviso ao se aproximar do limite de 8 caracteres
- **Indicador de status de cálculo** — animação de "aguardando" (âmbar) e confirmação de "calculado" (verde)
- **Botão de limpar** — aparece dinamicamente ao digitar e reseta todos os estados
- **Layout responsivo** — adaptado para telas móveis e desktops

---

## 🔐 Algoritmo

O passcode é calculado pelo algoritmo padrão utilizado pela rede APRS-IS, derivado do indicativo da estação em maiúsculas:

```javascript
function calcPasscode(callsign) {
  var i = 0, code = 29666;
  while (i < callsign.length) {
    code = code ^ (callsign.charCodeAt(i) * 256);
    code = code ^ (callsign.charCodeAt(i + 1) || 0);
    i += 2;
  }
  return code & 32767;
}
```

O resultado é um número inteiro de 0 a 32767 vinculado ao indicativo informado.

---

## 🎨 Design

- **Tema:** terminal retrô escuro com scanlines e glow verde/âmbar
- **Paleta:** fundo `#080c0e`, acento `#00e5a0` (verde), destaque `#ffb347` (âmbar)
- **Tipografia:** [Orbitron](https://fonts.google.com/specimen/Orbitron) (títulos e valores) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) (interface)
- **Acessibilidade:** contraste alto, sem dependência de distinção vermelho/verde para informações críticas

---

## 🏗️ Estrutura do Projeto

```
.
└── index.html   # Aplicação completa — HTML, CSS e JavaScript em arquivo único
```

Sem frameworks, sem build step, sem dependências de runtime. Basta abrir o arquivo em qualquer navegador moderno.

---

## 🚀 Deploy

O projeto pode ser hospedado em qualquer serviço de hospedagem estática (Cloudflare Pages, GitHub Pages, Nginx, Apache etc.).

Exemplo com Nginx:

```nginx
server {
    listen 80;
    server_name aprs.dvbr.net;
    root /var/www/aprs;
    index index.html;
}
```

---

## 🔧 Compatibilidade

| Recurso | Suporte |
|---|---|
| Clipboard API (`navigator.clipboard`) | Navegadores modernos (HTTPS) |
| Fallback `execCommand` | Navegadores legados / HTTP |
| Layout responsivo | Mobile e desktop |
| JavaScript necessário | Sim (cálculo client-side) |

---

## 📻 Sobre o APRS-IS

O **APRS-IS** é a infraestrutura de internet que interliga os gateways APRS ao redor do mundo, permitindo o rastreamento e troca de dados entre estações de radioamador. Para se conectar a um servidor APRS-IS com um software cliente (como Xastir, YAAC, Direwolf etc.), é necessário um passcode derivado do próprio indicativo da estação.

Referência: [www.aprs-is.net](http://www.aprs-is.net)

---

## 👤 Autor

**Daniel K. — PU5KOD**

- Site: [dvbr.net](https://dvbr.net)
- WhatsApp: [Contato](https://api.whatsapp.com/send?phone=5541991912000)

---

## 📄 Licença

Este projeto é disponibilizado para uso livre pela comunidade de radioamadores.
Sinta-se à vontade para adaptar e redistribuir, mantendo a atribuição ao autor original.
