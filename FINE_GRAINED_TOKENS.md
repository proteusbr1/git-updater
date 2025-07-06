# Fine-grained Personal Access Tokens para Repositórios Privados

## Visão Geral

Esta modificação do plugin Git Updater adiciona suporte para **Fine-grained personal access tokens** do GitHub, permitindo que o plugin funcione com repositórios privados usando a nova API de autenticação do GitHub.

## O que mudou

### Diferenças entre tokens clássicos e Fine-grained

- **Tokens clássicos**: Usam formato `token YOUR_TOKEN` no header Authorization
- **Fine-grained tokens**: Usam formato `Bearer YOUR_TOKEN` no header Authorization

### Arquivos modificados

1. **`src/Git_Updater/Traits/Basic_Auth_Loader.php`**

   - Adicionado suporte para detectar se deve usar formato "Bearer" ou "token"
   - Adicionado campo `use_fine_grained` nos credentials

2. **`src/Git_Updater/API/GitHub_API.php`**

   - Adicionado campo de configuração para Fine-grained tokens
   - Atualizado texto explicativo nas configurações
   - Adicionado suporte para salvar preferência de Fine-grained tokens

3. **`src/Git_Updater/Settings.php`**
   - Adicionado callback para checkbox Fine-grained
   - Adicionado suporte para configurações individuais por repositório
   - Preservação das configurações durante limpeza de cache

## Como usar

### Configuração Global

1. Vá para **Configurações > Git Updater** no painel administrativo
2. Na aba **GitHub**, você verá:
   - Campo "GitHub.com Access Token" - onde você insere seu token
   - Checkbox "Use Fine-grained Personal Access Token" - marque para usar formato Bearer

### Configuração por Repositório

Para repositórios privados individuais, você verá:

- Campo de token específico para cada repositório
- Checkbox "Use Fine-grained token for this repository" para cada repositório privado

### Instalação Remota

Ao usar a funcionalidade de instalação remota:

1. Insira o token no campo "GitHub Access Token"
2. Marque "Use Fine-grained Token" se estiver usando um Fine-grained personal access token

## Criando um Fine-grained Personal Access Token

1. Vá para **GitHub.com > Settings > Developer settings > Personal access tokens > Fine-grained tokens**
2. Clique em "Generate new token"
3. Configure as permissões necessárias:

   - **Repository permissions**:
     - Contents: Read (para baixar arquivos)
     - Metadata: Read (para informações do repositório)
     - Pull requests: Read (se usar pull requests)
   - **Account permissions**: Nenhuma necessária para repositórios básicos

4. Selecione os repositórios específicos que precisam de acesso
5. Gere o token e copie-o

## Vantagens dos Fine-grained Tokens

- **Segurança aprimorada**: Acesso limitado apenas aos repositórios necessários
- **Controle granular**: Permissões específicas por repositório
- **Auditoria melhorada**: Logs detalhados de uso
- **Conformidade**: Melhor para organizações com políticas de segurança rígidas

## Resolução de Problemas

### Erro 401 - Unauthorized

Se você receber erro 401:

1. Verifique se o token está correto
2. Confirme se marcou a opção "Use Fine-grained Token" se estiver usando esse tipo
3. Verifique se o token tem permissões adequadas para o repositório

### Erro 403 - Forbidden

Se você receber erro 403:

1. Verifique se o token tem acesso ao repositório específico
2. Confirme se as permissões incluem "Contents: Read" e "Metadata: Read"
3. Verifique se o repositório não foi removido do escopo do token

### Token não funciona

1. Confirme se está usando o formato correto (marque/desmarque "Use Fine-grained Token")
2. Verifique se o token não expirou
3. Teste o token manualmente com curl:
   ```bash
   curl -H "Authorization: Bearer YOUR_TOKEN" https://api.github.com/repos/owner/repo
   ```

## Migração de Tokens Clássicos

Se você já usa tokens clássicos:

1. Seus tokens existentes continuarão funcionando
2. Para migrar para Fine-grained tokens:
   - Crie um novo Fine-grained token
   - Substitua o token antigo pelo novo
   - Marque a opção "Use Fine-grained Token"
   - Teste a funcionalidade

## Compatibilidade

- **WordPress**: 5.9+
- **PHP**: 8.0+
- **GitHub API**: v3 e v4
- **Tokens suportados**: Clássicos e Fine-grained

## Notas Técnicas

- O plugin detecta automaticamente o tipo de token baseado na configuração
- Configurações são salvas no banco de dados do WordPress
- Suporte para instalações multisite
- Cache automático para evitar chamadas desnecessárias à API

---

_Esta modificação mantém total compatibilidade com tokens clássicos existentes, permitindo migração gradual para Fine-grained tokens._
