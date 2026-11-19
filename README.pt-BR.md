*[Read in English](README.md)*

# Relógio Digital - ESP32 CYD 2.8"

Relógio digital com previsão do tempo e alarmes para a placa **ESP32 CYD** (ESP32-2432S028R).

## Funcionalidades

- **Portal captive** para configuração WiFi (sem editar código)
- Sincronização de horário via NTP
- Display com hora grande (7-segment), segundos, data completa
- **Previsão do tempo** via OpenWeatherMap (clima atual + 3 dias)
- **Até 5 alarmes** configuráveis pela interface web
- Alarme visual: tela pisca amarelo/vermelho até tocar no display
- Alarme sempre dispara com brilho 100%
- **Controle de brilho** com 4 níveis (25%, 50%, 75%, 100%)
- **Dois temas**: escuro e claro (alternável por botão)
- Tela de visualização dos alarmes ativos
- WebUI acessível pelo IP na rede local (exibe configurações atuais)
- Coordenadas geográficas (lat/lon) para precisão na previsão
- Tratamento de acentos UTF-8 (exibe sem acentos no display)
- Configurações salvas na flash (sobrevive a reboot)
- Toque longo (3s): reseta tudo e volta ao modo configuração

## Como usar

### 1. Gravar na placa

```bash
pio run -t upload --upload-port COM6
```

### 2. Configurar (primeira vez)

1. A placa cria a rede WiFi **Relogio-CYD** (senha: `12345678`)
2. Conecte seu celular/PC nessa rede
3. Abra **http://192.168.4.1** no navegador
4. Preencha: SSID, senha, cidade, latitude, longitude, fuso horário, API key e alarmes
5. Clique "Salvar e Conectar"

### 3. Reconfigurar (já conectado)

Acesse o IP exibido no rodapé do display (ex: `http://192.168.0.17`) pelo navegador na mesma rede. A página mostra as configurações atuais já preenchidas.

### 4. Interação pelo display

| Botão | Ação |
|---|---|
| **ESC/LUZ** | Alterna tema escuro/claro |
| **AL** | Mostra tela com alarmes ativos |
| **75%** | Cicla brilho: 25% → 50% → 75% → 100% |
| **Segura 3s** | Reseta config, volta ao modo AP |

### 5. Previsão do tempo

Crie uma conta gratuita em [openweathermap.org](https://openweathermap.org/api) e cole a API key na configuração. Grátis: 1000 chamadas/dia. O relógio consulta a cada 10 minutos.

Para coordenadas da sua cidade, use [Google Maps](https://maps.google.com) (clique direito → "O que há aqui?" mostra lat/lon).

## Hardware

- ESP32 CYD 2.8" (ESP32-2432S028R)
- Display ILI9341 240x320 integrado
- Touch resistivo XPT2046 integrado

## Estrutura

```
CYD/
├── platformio.ini    # Config PlatformIO + pinos do display
├── src/
│   └── main.cpp      # Código principal
├── .gitignore
└── README.md
```

## Bibliotecas

| Biblioteca | Função |
|---|---|
| TFT_eSPI | Driver do display ILI9341 |
| XPT2046_Touchscreen | Driver do touch |
| ArduinoJson | Parse da API OpenWeatherMap |
| WebServer | Interface web de configuração |
| DNSServer | Captive portal |
| Preferences | Salvar config na flash |

## Compilação

```bash
pio run              # Compilar
pio run -t upload    # Gravar na placa
pio device monitor   # Monitor serial (debug)
```
