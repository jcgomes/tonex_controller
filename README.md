# 1. Tonex Controller para Raspberry Pi

Controlador touchscreen para o pedal IK Multimedia Tonex, desenvolvido para Raspberry Pi com tela de 7".

![Tonex Controller](screenshot.png)

## 📋 Sobre o Projeto

Este projeto implementa um controlador completo para o pedal Tonex da IK Multimedia, utilizando um Raspberry Pi com tela touchscreen de 7". O controlador permite:

- Seleção de presets via interface touch
- Visualização do preset atual com nome e número
- Controle MIDI via USB
- Interface otimizada para tela de 7" (1024x600)

O projeto foi inspirado no [TonexOneController](https://github.com/Builty/TonexOneController) da comunidade, mas adaptado para Raspberry Pi com Python/Pygame.

## 🎯 Status Atual

✅ **Funcionalidades implementadas:**
- Conexão MIDI com o Tonex (via python-rtmidi)
- Seleção de 20 presets via touch/teclado
- Grid 4x5 com nomes dos presets
- Feedback visual do preset selecionado
- Interface em tela cheia para 7"
- Atalhos de teclado (1-0 para presets 1-10)

🚧 **Em desenvolvimento:**
- Interface inspirada no Tonex One Controller
- Exibição de BPM
- Controle de efeitos (Noise Gate, Amp, Cab, Compressor, Modulação, Delay, Reverb)
- Banco de presets
- Imagens de amplificadores/pedais

## 🛠️ Hardware

- Raspberry Pi (testado no Pi 3 e 4)
- Tela LCD 7" com touch (1024x600)
- Cabo USB para conexão com o Tonex
- (Opcional) Footswitches via GPIO

## 📦 Instalação

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/tonex-controller-rpi.git
cd tonex-controller-rpi
```

# 2. Dependências do sistema
```bash
sudo apt-get update
sudo apt-get install -y python3-pygame python3-pil python3-rtmidi
pip3 install pygame pyusb pillow python-rtmidi
```

# 3. Permissões USB

## Regra udev permanente
```bash
sudo nano /etc/udev/rules.d/99-tonex.rules
```

## Conteúdo do arquivo
```text
SUBSYSTEM=="usb", ATTRS{idVendor}=="1963", ATTRS{idProduct}=="0068", MODE="0666"
```

## Recarregue as regras
```bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

# 4. Verifique a conexão
```bash
# Lista dispositivos USB
lsusb | grep -i ik

# Deve mostrar: Bus 001 Device 004: ID 1963:0068 IK Multimedia ToneX

# Testa conexão MIDI
python3 test_midi.py
```
# 5. Execute o App
```bash
python3 main.py
```
# 🎮 Como Usar
## Navegação Touch
- Toque em qualquer preset para selecioná-lo
- O preset selecionado fica destacado em azul

## Atalhos de Teclado
- 1 a 9: Seleciona presets 1 a 9
- 0: Seleciona preset 10
- ESC: Sai do aplicativo

## Conexão com o Tonex
- Conecte o Tonex via USB antes de iniciar o app
- O app detecta automaticamente o dispositivo MIDI
- Status "Conectado" aparece no canto superior direito

# 🏗️ Estrutura do Projeto
```text
tonex-controller-rpi/
├── main.py               # Aplicação principal com interface
├── tonex_midi.py         # Classe de comunicação MIDI
├── test_midi.py          # Teste de conexão MIDI
├── find_tonex.py         # Utilitário para encontrar o Tonex
├── assets/               # Imagens e recursos
│   └── amps/             # Imagens de amplificadores
└── README.md
```

# 🔧 Arquitetura do Código
## Classe TonexController
### Gerencia toda a aplicação:
- Inicialização do Pygame e MIDI
- Interface gráfica (grid de presets)
- Eventos de touch e teclado
- Conexão com o Tonex

## Classe TonexMIDI
### Gerencia a comunicação MIDI:
- Conexão via python-rtmidi
- Envio de Program Change (seleção de presets)
- Detecção automática do Tonex

## Interface Gráfica
- Grid 4x5 adaptativo para 1024x600
- Botões com cores diferenciadas (selecionado vs normal)
- Cabeçalho com título e status
- Rodapé com instruções e preset atual

# 📝 Notas de Desenvolvimento
## IDs USB do Tonex
O Tonex foi identificado com os seguintes IDs:
- Vendor ID: 1963
- Product ID: 0068

## Comunicação MIDI
O Tonex se apresenta como dispositivo MIDI com o nome:
ToneX:ToneX MIDI 1 20:0

## Comandos MIDI
- Program Change (seleção de preset): [0xC0, preset-1]
- Control Change (parâmetros): [0xB0, cc, value]

# 🤝 Contribuição
Sinta-se à vontade para contribuir com o projeto!

1. Fork o repositório
2. Crie uma branch para sua feature (git checkout -b feature/nova-feature)
3. Commit suas mudanças (git commit -m 'Adiciona nova feature')
4. Push para a branch (git push origin feature/nova-feature)
5. Abra um Pull Request

# 📄 Licença
Este projeto está sob a licença Apache 2.0.

# 🙏 Agradecimentos
- TonexOneController (https://github.com/Builty/TonexOneController) - Inspiração para o projeto
- Comunidade IK Multimedia - Pelo suporte e documentação
- Comunidade Raspberry Pi - Pela plataforma incrível

