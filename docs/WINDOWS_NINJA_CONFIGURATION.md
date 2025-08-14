# Configuração do Ninja no Windows para React Native

## Problema

No Windows, ao compilar aplicações React Native com a nova arquitetura (Fabric) habilitada, você pode encontrar o seguinte erro:

```
ninja: error: Stat(...): Filename longer than 260 characters
```

Este erro ocorre devido à limitação do Windows de 260 caracteres para o tamanho do caminho de arquivo, que é excedido durante o processo de compilação com CMake.

## Solução

### 1. Configuração Única no `local.properties`

Implementamos uma solução simples e direta usando apenas o arquivo `local.properties`:

```properties
# Configuração específica para Windows - caminho do ninja para resolver problema de path longo
# Esta configuração resolve o erro "Filename longer than 260 characters" no Windows
windows.ninja.path=C:\\Users\\Douglas\\AppData\\Local\\Android\\Sdk\\cmake\\3.22.1\\bin\\ninja.exe

# Configuração para aumentar o limite de tamanho do path no CMake
cmake.object.path.max=1024
```

### 2. Uso no `build.gradle`

O arquivo `build.gradle` de cada projeto Android agora usa as configurações do `local.properties`:

```gradle
// Importa as configurações compartilhadas
apply from: "../gradle/ninja-config.gradle"

// No defaultConfig
externalNativeBuild {
    cmake {
        def config = getNinjaConfig()
        if (config.ninjaPath) {
            arguments "-DCMAKE_MAKE_PROGRAM=${config.ninjaPath}", "-DCMAKE_OBJECT_PATH_MAX=${config.objectPathMax}"
        }
    }
}
```

## Como Personalizar

### Para outros usuários Windows

1. **Edite o arquivo `local.properties`** no diretório `android/` do projeto
2. **Atualize o caminho do ninja** para o seu sistema:

    ```properties
    windows.ninja.path=C:\\Users\\SEU_USUARIO\\AppData\\Local\\Android\\Sdk\\cmake\\VERSÃO\\bin\\ninja.exe
    ```

3. **Verifique a versão do CMake** instalada no seu Android SDK:
    - Abra o Android Studio
    - Vá para `File > Settings > Appearance & Behavior > System Settings > Android SDK`
    - Na aba `SDK Tools`, verifique a versão do CMake instalada

### Para diferentes versões do CMake

Se você tiver uma versão diferente do CMake, atualize o caminho:

```properties
# Exemplo para CMake 3.24.1
windows.ninja.path=C:\\Users\\SEU_USUARIO\\AppData\\Local\\Android\\Sdk\\cmake\\3.24.1\\bin\\ninja.exe
```

## Referências

Esta solução é baseada na discussão do GitHub sobre o problema de path longo no Windows:

-   [Issue #424 - react-native-safe-area-context](https://github.com/AppAndFlow/react-native-safe-area-context/issues/424#issuecomment-2454869033)

## Benefícios da Implementação

1. **Simplicidade**: Configuração em um único local - `local.properties`
2. **Flexibilidade**: Fácil de personalizar para diferentes usuários e versões
3. **Manutenibilidade**: Mudanças de configuração em um único arquivo
4. **Compatibilidade**: Funciona tanto em Windows quanto em outros sistemas
5. **Padrão Android**: Usa o arquivo padrão para configurações locais
6. **Sem Duplicação**: Não há configurações duplicadas em múltiplos arquivos

## Notas Importantes

-   Esta configuração é específica para Windows
-   O fallback garante que a compilação funcione mesmo se as propriedades não estiverem definidas
-   Aumentar `CMAKE_OBJECT_PATH_MAX` para 1024 ajuda a contornar a limitação de 260 caracteres do Windows
