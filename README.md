# Apartamento Virtual Multiplayer

Protótipo web de apartamento interativo com personagens sincronizados em tempo real pelo Firebase Realtime Database.

## Recursos
- Planta esquemática interativa com sala, quartos, cozinha, jantar e escritório.
- Personagem controlável por setas e interações com móveis.
- Apelido e nome de sala compartilhada.
- Firebase Authentication anônimo e sincronização de posições via Realtime Database.
- Publicação automática no GitHub Pages via GitHub Actions.

## Configuração Firebase
1. Crie um projeto em https://console.firebase.google.com/
2. Adicione um app Web e copie a configuração.
3. Em **Build → Authentication → Sign-in method**, habilite **Anonymous**.
4. Em **Build → Realtime Database**, crie um banco e copie a URL do banco.
5. No site, abra **Configuração Firebase**, cole o objeto de configuração Web incluindo `databaseURL`, aplique e conecte.

### Regras recomendadas do Realtime Database
Em Realtime Database → Rules, use:
```json
{
  "rules": {
    "rooms": {
      "$room": {
        "players": {
          ".read": "auth != null",
          "$uid": {
            ".write": "auth != null && auth.uid === $uid"
          }
        }
      }
    }
  }
}
```
A aplicação usa o UID de autenticação como ID do jogador. Não use regras públicas com `".read": true` ou `".write": true`.

## Publicação
O workflow `.github/workflows/pages.yml` publica automaticamente a raiz do repositório no GitHub Pages quando houver push para `main`.
Se necessário, acesse **Settings → Pages** e escolha **GitHub Actions** como fonte de publicação.

Após a primeira execução bem-sucedida, o endereço será:
https://richielybmp.github.io/apartamento-virtual-multiplayer/

## Limitações atuais
- A planta é esquemática, não uma reprodução fiel da imagem arquitetônica original.
- Movimentos e apelidos são sincronizados; animações/ações dos móveis ainda são locais.
- Para uma aplicação pública mais robusta, considere limitar abuso, validar frequência de atualizações e configurar controles de acesso adicionais.
