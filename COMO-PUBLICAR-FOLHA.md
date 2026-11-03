# Folha: HTML, Android e publicação na Google Play

Preparado em 3 de novembro de 2026.

## Arquivos entregues

- **Folha.html**: edição em um único arquivo, com CSS, JavaScript, PDF.js, gerador de PDF e fontes incorporados. Abra no Chrome ou Edge. O reconhecimento de páginas escaneadas baixa componentes e idiomas pela internet. O comportamento de armazenamento ao abrir arquivos locais varia conforme o navegador; a edição Android tem um armazenamento próprio.
- **Folha-codigo-e-Android.zip**: projeto Android, código web editável, dependências declaradas, arquivo de versões travadas e este guia. O HTML único também está incluído.

O app Android usa Capacitor 8.6.0 e incorpora os arquivos localmente. Não depende do site privado nem exige login no ChatGPT. Abrir o site no navegador e instalar este app são experiências com estantes separadas; os documentos não são transferidos automaticamente.

## O que foi implementado e verificado

O HTML único foi testado abrindo diretamente como arquivo local: importação de PDF, leitura adaptada, renderização da página original e exportação de uma cópia com fontes incorporadas. A edição web já havia sido testada com OCR em português, estante local e correções de texto.

O projeto Android foi gerado com `cap add android`, sincronizado com os três plugins nativos e verificado quanto à sintaxe JavaScript. A exportação usa o compartilhamento nativo do Android, com um arquivo temporário no cache do app. O botão Voltar fecha diálogos, retorna à estante ou minimiza o app. A cópia automática de segurança do Android foi desabilitada na configuração.

**O projeto Android ainda não foi compilado nem executado em um Android real ou emulador.** Não há Android Studio/SDK instalado no ambiente usado para esta entrega. Não está incluído um APK ou AAB assinado. A aprovação pela loja depende da avaliação do Google.

## 1. Preparar o computador

1. Instale o [Node.js](https://nodejs.org/) versão 22 ou superior.
2. Instale o [Android Studio](https://developer.android.com/studio), versão 2025.2.1 ou superior, conforme os requisitos do Capacitor 8. Use o JDK integrado ao Android Studio.
3. Na primeira abertura, conclua a instalação dos componentes recomendados.
4. Abra **Tools → SDK Manager**. Instale **Android 16 / API 36**, Android SDK Build-Tools, Platform-Tools e Command-line Tools.
5. Extraia o ZIP em uma pasta simples, por exemplo `C:\Projetos\Folha`. Evite trabalhar dentro do ZIP.

Referência: [ambiente do Capacitor](https://capacitorjs.com/docs/getting-started/environment-setup).

## 2. Abrir o projeto Android

Dentro da pasta extraída, abra o terminal na pasta `folha-android`, aquela que contém `package.json`.

```powershell
npm ci
npm run android:sync
npm run android:open
```

O último comando abre a pasta `android` no Android Studio. Aguarde o Gradle terminar a sincronização. Se surgir um aviso de SDK ausente, instale a versão solicitada pelo SDK Manager.

O projeto usa `compileSdkVersion = 36` e `targetSdkVersion = 36`. O mínimo configurado é API 24, mas é necessário manter o Android System WebView atualizado. O suporte real em versões antigas precisa ser testado.

O identificador inicial é **com.asael.folha**. Confirme se deseja usá-lo e se está disponível antes da primeira publicação. Para mudar, ajuste `appId` em `capacitor.config.json`, `namespace` e `applicationId` em `android/app/build.gradle`, e o pacote Java de `MainActivity` pelo Refactor do Android Studio. Depois execute `npm run android:sync`.

## 3. Testar no celular

1. Ative as **Opções do desenvolvedor** e a **Depuração USB** no Android.
2. Conecte o celular por USB e aceite a autorização de depuração na tela.
3. No Android Studio, selecione o aparelho na barra superior.
4. Clique em **Run ▶**.
5. Abra um PDF real com o seletor do Android, inclusive um PDF escaneado.
6. Teste fontes, margens, temas, avanço de página, estante após fechar e reabrir, correção manual, OCR, botão Voltar e rotação da tela.
7. Em **Salvar PDF**, confira se o menu de compartilhamento aparece e se o PDF abre corretamente no destino escolhido. As opções dependem dos apps instalados; escolha um app de arquivos ou Drive para guardar uma cópia.
8. Teste sem internet: a leitura comum deve funcionar. OCR pode precisar baixar componentes/idiomas. Teste também Android 15/16 para confirmar margens junto às barras do sistema.

O texto adaptado não preserva imagens, tabelas ou a paginação original. O original continua acessível no leitor. Arquivos locais são limitados a 60 MB e 1.500 páginas. Limpar os dados do app ou desinstalá-lo remove sua estante local; PDFs exportados para outros apps são independentes.

## 4. Gerar o AAB assinado para a loja

1. No Android Studio, abra **Build → Generate Signed App Bundle or APK**.
2. Escolha **Android App Bundle** e clique em **Next**.
3. Na primeira publicação, clique em **Create new** para criar sua chave de upload.
4. Escolha o arquivo `.jks`, defina alias e senhas e preencha os dados solicitados.
5. Guarde uma cópia segura da chave e das senhas fora do projeto. Não as publique nem inclua em ZIPs de código.
6. Selecione a variante **release** e finalize.
7. Use **Locate** para encontrar o `.aab` gerado. O Android Studio costuma usar a pasta `android/app/release`; o caminho pode variar conforme a versão.

O APK serve para instalação direta; o **AAB assinado** é o pacote de distribuição para a Google Play. Fonte: [assinatura de apps](https://developer.android.com/studio/publish/app-signing).

## 5. Preparar a ficha na Google Play

Crie ou acesse sua conta no [Google Play Console](https://play.google.com/console). Conclua a verificação exigida para sua conta e crie o aplicativo Folha.

Preencha nome, descrições, categoria, contato de suporte, ícone, capturas reais do app Android, classificação indicativa, público-alvo, anúncios e informações de acesso. O app fornecido não tem login nem anúncios. Use somente declarações que correspondam à versão que você efetivamente publicar.

**Privacidade:** há um texto-base em `docs/POLITICA-DE-PRIVACIDADE-RASCUNHO.md`. Preencha o responsável e o contato, revise a versão final e publique em uma URL HTTPS pública, acessível sem login. Não use o link privado do site como política pública. A seção Segurança dos dados deve corresponder ao app e às bibliotecas realmente incluídas. O OCR faz downloads de jsDelivr e Project Naptha; esses provedores podem receber IP e dados técnicos da conexão. Os PDFs não são enviados ao seu servidor pelo código atual. O usuário pode compartilhar voluntariamente o PDF exportado com outros apps.

Confira [a política de dados do usuário](https://support.google.com/googleplay/android-developer/answer/10144311?hl=pt-BR) antes de enviar. Não foi preenchida nenhuma declaração no Play Console por você.

## 6. Testar pela loja e solicitar publicação

1. Envie o AAB para **Teste interno** e verifique a instalação pela Google Play.
2. Corrija problemas apontados no relatório de pré-lançamento.
3. Quando aplicável à sua conta, faça o **teste fechado** e solicite acesso à produção.
4. Para contas pessoais criadas após 13/11/2023, a regra consultada exige pelo menos **12 testadores inscritos continuamente por 14 dias** antes do pedido de acesso à produção. A conclusão do teste não garante aprovação automática. [Regra oficial](https://support.google.com/googleplay/android-developer/answer/14151465?hl=pt-BR).
5. Com acesso à produção, complete as declarações, selecione países e envie para revisão.

Na data deste guia, novos apps comuns devem mirar **Android 16 / API 36 ou superior**, requisito já configurado no projeto. Confirme novamente na data do envio: [requisitos oficiais de API](https://developer.android.com/google/play/requirements/target-sdk).

## Atualizar o código depois

Edite `www/index.html`, `www/style.css` e `www/app.js`. A integração Android está em `src/native.js`.

```powershell
npm run check
npm run android:sync
npm run android:open
```

Antes de publicar uma atualização, aumente `versionCode` em `android/app/build.gradle` e ajuste `versionName`. Gere outro AAB com a mesma identidade de aplicativo e a chave de upload correspondente. Não substitua o app publicado por um pacote com outro identificador.

O site já publicado não foi alterado nesta entrega. O ZIP é uma edição independente. Licenças de bibliotecas e fontes estão em `www/vendor`.
