# Frontend Mobile - Sistema de Automação de Abastecimento de Frotas

## Propósito

Este diretório contém o código do aplicativo mobile da aplicação de automação de abastecimento de frotas. O frontend mobile é responsável por:

- **Aplicação Móvel**: App para motoristas e gestores de frota
- **Registro de Abastecimentos**: Permite aos motoristas registrar abastecimentos em campo
- **Consultas Rápidas**: Visualização de informações do veículo e histórico
- **Notificações**: Alertas e notificações push
- **Offline First**: Funcionalidades disponíveis mesmo sem conexão
- **Geolocalização**: Registro automático de localização do abastecimento

## Estrutura Planejada

```
src/frontend-mobile/
├── src/
│   ├── components/     # Componentes reutilizáveis
│   ├── screens/        # Telas da aplicação
│   ├── navigation/     # Navegação e rotas
│   ├── services/       # Serviços de API e integrações
│   ├── store/          # Gerenciamento de estado
│   ├── hooks/          # Custom hooks
│   ├── utils/          # Funções utilitárias
│   ├── assets/         # Imagens, fontes, ícones
│   ├── styles/         # Estilos e temas
│   └── config/         # Configurações do app
├── android/            # Código nativo Android
├── ios/                # Código nativo iOS
├── tests/              # Testes unitários e E2E
└── README.md           # Este arquivo
```

## Tecnologias Previstas

- **Framework**: React Native ou Flutter
- **Navegação**: React Navigation
- **Gerenciamento de Estado**: Redux, MobX ou Context API
- **Armazenamento Local**: AsyncStorage, SQLite ou Realm
- **Requisições HTTP**: Axios
- **Mapas e Geolocalização**: React Native Maps, Geolocation API
- **Notificações Push**: Firebase Cloud Messaging (FCM)

## Funcionalidades Principais

1. **Autenticação**
   - Login de motoristas
   - Biometria (Touch ID / Face ID)
   - Token de sessão

2. **Registro de Abastecimento**
   - Scanner de código de barras / QR Code
   - Entrada manual de dados
   - Captura de foto do comprovante
   - Localização GPS automática
   - Modo offline

3. **Consultas**
   - Dados do veículo
   - Histórico de abastecimentos
   - Média de consumo

4. **Notificações**
   - Alertas de manutenção
   - Lembretes
   - Mensagens do sistema

## Plataformas Suportadas

- **Android**: 6.0 (API 23) ou superior
- **iOS**: 12.0 ou superior

## Como Começar

1. Instalar dependências: `npm install` ou `yarn install`
2. Configurar variáveis de ambiente
3. Para Android:
   - `npx react-native run-android` ou `flutter run`
4. Para iOS:
   - `cd ios && pod install && cd ..`
   - `npx react-native run-ios` ou `flutter run`

## Status

🚧 **Em construção** - Aguardando primeira implementação
