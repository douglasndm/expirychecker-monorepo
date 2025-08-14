# ExpiryChecker Monorepo

Este é um monorepo contendo múltiplos projetos React Native relacionados ao ExpiryChecker.

## Estrutura do Projeto

```
expirychecker-monorepo/
├── packages/
│   ├── expirychecker/          # App principal do ExpiryChecker
│   ├── teams/                  # App para equipes de negócio
│   └── shared/                 # Bibliotecas e utilitários compartilhados
├── docs/                       # Documentação do projeto
├── gradle.properties           # Configurações globais do Gradle
└── README.md                   # Este arquivo
```

## Configuração para Windows

### Problema de Path Longo

No Windows, ao compilar aplicações React Native com a nova arquitetura (Fabric), você pode encontrar o erro:

```
ninja: error: Stat(...): Filename longer than 260 characters
```

### Solução Implementada

Criamos uma solução simples e direta para configurar o caminho do ninja e resolver problemas de path longo:

1. **Arquivo de Configuração**: `local.properties` no diretório `android/` de cada projeto
2. **Configuração Automática**: Os projetos Android usam essas configurações automaticamente
3. **Personalização**: Fácil de configurar para diferentes usuários e versões

### Como Configurar

1. **Edite o arquivo `local.properties`** no diretório `android/` do projeto
2. **Atualize o caminho do ninja** para o seu sistema:
    ```properties
    windows.ninja.path=C:\\Users\\SEU_USUARIO\\AppData\\Local\\Android\\Sdk\\cmake\\VERSÃO\\bin\\ninja.exe
    ```

### Documentação Detalhada

Para instruções completas, consulte:

-   [Configuração do Ninja no Windows](docs/WINDOWS_NINJA_CONFIGURATION.md)

## Desenvolvimento

### Pré-requisitos

-   Node.js 18+
-   React Native CLI
-   Android Studio (para builds Android)
-   Xcode (para builds iOS)

### Instalação

```bash
# Instalar dependências
pnpm install

# Para um projeto específico
cd packages/teams
pnpm install
```

### Build

```bash
# Build Android
cd packages/teams/android
./gradlew assembleDebug

# Build iOS
cd packages/teams/ios
pod install
```

## Contribuição

1. Faça fork do projeto
2. Crie uma branch para sua feature
3. Commit suas mudanças
4. Push para a branch
5. Abra um Pull Request

## Licença

Este projeto está sob licença proprietária.
