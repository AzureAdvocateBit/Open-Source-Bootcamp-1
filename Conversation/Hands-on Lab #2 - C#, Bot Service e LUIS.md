# Hands-on Lab #2 - C#, Bot Service e LUIS #

Nesse Hands-on Lab, criaremos um bot usando o Azure Bot Service, que usa o Language Understanding (LUIS) para entender o usuário. Ao criar um bot usando linguagem natural, o bot determina o que um usuário quer fazer identificando sua intenção (Intent). Essa intenção é determinada a partir de entradas faladas ou textuais, que por sua vez podem ser mapeados para ações que os determinadas no código fonte do Bot. Por exemplo, um bot de tomada de notas reconhece a intenção Notes.Create para invocar a funcionalidade para criar uma nota. Um bot também pode precisar extrair entidades (Entities), que são palavras importantes em expressões. Nesse exemplo do bot de tomada de notas, a entidade do título (Notes.title) identifica o título de cada nota.

## Criar um bot com reconhecimento de fala com o Bot Service ##

Para criar o seu bot; Faça login no portal Azure, selecione  **Criar novo recurso**  na barra de menu e selecione  **AI + Serviços Cognitivos**.

  ![img](images/00.png)

Você pode navegar pelas sugestões ou procurar por **Web App Bot.**

  ![img](images/01.png)

Uma vez selecionada, a tela do Serviço Bot deve aparecer; que será familiar para os usuários dos serviços Azure. Para aqueles que não são, aqui você pode especificar informações sobre o seu serviço para o Bot Service usar na criação do seu bot, como onde ele vai ser hospedado, qual a assinatura e assim por diante. Na  **barra de serviço**  do  **Bot** , forneça as informações necessárias e clique em  **Criar**. Isso cria e implanta o Bot Service e o aplicativo LUIS no Azure. Alguns campos interessantes:

- Defina o  **nome da aplicação**  para o  **nome**  do seu bot. O nome é usado como subdomínio quando o seu bot é implantado na nuvem (por exemplo, mynotesbot.azurewebsites.net). Este nome também é usado como o nome do aplicativo LUIS associado ao seu bot. Copie-o para usar mais tarde, para encontrar o aplicativo LUIS associado ao bot.

- Selecione a assinatura, o grupo de recursos, o plano de hospedagem e a localização.
- Para preços, você pode escolher o  **nível de preços**  gratuito. Você pode voltar e mudar isso a qualquer momento se precisar.
- Para essa demo, selecione o template **Language understanding (C#)** no campo do  **Bot**   **Template**.
- Para o ultimo campo obrigatório, escolha o  **Azure Storage**  onde você deseja armazenar o estado de conversação do seu bot. Pense nisso como onde o bot armazena o status da conversa de cada usuário.

  ![img](images/02.png)

Agora que você está pronto, você pode clicar em  **Create**. O Azure definirá a criação do seu bot, incluindo os recursos necessários para sua operação e uma conta LUIS para hospedar seu modelo de linguagem natural. Uma vez concluído, você receberá uma notificação através do sino no canto superior direito do portal Azure.

Em seguida, vamos validar se o serviço de bot foi implantado.

- Clique em Notificações (o ícone de sino que está localizado ao longo da borda superior do portal Azure). A notificação mudará de  **implantação iniciada**  para **implantação bem-sucedida**.

- Depois que a notificação mudar para a  **Implantação bem-sucedida** , clique em  **Ir para recurso**  sobre essa notificação.

## Experimente o bot ##

Então, agora você deve ter um bot funcionado. Vamos testar.

Uma vez que o bot esteja registrado, clique  **em Testar no Chat da Web**  para abrir o painel de bate-papo da Web. Digite &quot;hello&quot; no Chat na Web.

![img](images/03.png)

O bot responde dizendo: &quot;You have reached Greeting. You said: hello&quot;. Isso confirma que o bot recebeu sua mensagem e passou para um aplicativo LUIS padrão que criamos. Este aplicativo LUIS padrão detectou uma intenção de saudação. **Note** : Ocasionalmente, a primeira mensagem ou duas após a inicialização talvez precise ser tentada novamente antes que o bot responda.

Legal! Você tem um bot em funcionamento! O bot padrão apenas conhece algumas coisas; Ele reconhece alguns cumprimentos, além de ajudar e cancelar. Na próxima seção, modificaremos o aplicativo LUIS para o nosso bot para adicionar algumas novas intenções para o nosso bot.

## Modificar o aplicativo LUIS ##

Faça login no www.luis.ai usando a mesma conta que você usa para fazer login no Azure. Clique em  **Meus aplicativos**. Se tudo correu bem, na lista de aplicativos, você encontrará o aplicativo com o mesmo nome que o  **nome**  do  **aplicativo digitado**  da lâmina do  **Bot Service**  quando você criou o seu bot.

Depois de abrir o aplicativo, você deve ver que ele tem quatro intenções: Cancel, Greeting, Help e None. Os três primeiros que já mencionamos.  **None**  é uma intenção especial no LUIS que captura &quot;todo o restante&quot;.

Para a nossa demo, vamos adicionar três intenções para o usuário: **Note.Create**  e  **Note.ReadAloud**. Convenientemente, um dos grandes recursos sobre LUIS são os domínios pré-construídos que podem ser usados ​​para inicializar sua aplicação, como é o caso do Note.

- Clique nos  **domínios pré-construídos**  no canto inferior esquerdo da página. Encontre os domínios  **Note.Create**  e  **Note.ReadAloud**.  e clique em  **Adicionar domínio**.
- Este tutorial não usa todas as intenções incluídas no domínio pré-construído  **Note**. Por conta disso, não é necessário selecionar as demais opções presentes dentro do dominio Note:
  - ShowNext
  - DeleteNoteItem
  - Confirm
  - Clear
  - CheckOffItem
  - AddToNote
  - Delete

- **IMPORTANTE:** as únicas intenções que devem permanecer no aplicativo LUIS são as intenções Note.ReadAloud, Note.Create, None, Help, Greeting e Cancel. Se eles ainda estão lá, seu aplicativo ainda funcionará, mas pode se comportar com mais frequência de forma inconsistente.

Como mencionado anteriormente, os Intents que agora adicionamos representam os tipos de coisas que esperamos que o usuário deseje que o bot faça. Uma vez que estes são pré-definidos, não temos que ajustar mais o modelo, então vamos pular diretamente para treinar e publicar seu modelo.

- Clique no botão  **Train**  no canto superior direito para treinar seu aplicativo. O treinamento leva tudo o que você inseriu no modelo, criando intenções e entidades, inserindo enunciados e rotulando-os e gera um modelo aprendido em máquina, tudo com um clique. Você pode testar seu aplicativo aqui no portal LUIS, ou seguir à publicação para que ele esteja disponível para o seu bot.

- Clique em  **Publish**  na barra de navegação superior para abrir a página  **Publish App**. Clique no botão de  **Publish to production host**. Após a publicação bem-sucedida, copie o URL exibido na coluna  **Endpoint**  na página  **Publicar aplicativo** , na linha que começa com o Nome do recurso Starter\_Key. Salve este URL para usar mais tarde no código do seu bot. A URL possui um formato semelhante a este exemplo: ```https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/xxxxxxxxxxxxxxxxx?subscription-key=xxxxxxxxxxxxxx3&amp;timezoneOffset=0&amp;verbose=true&amp;q=```

Seu aplicativo LUIS agora está pronto para o seu Bot. Se o usuário pedir para criar, excluir ou ler uma nota, a LUIS irá identificá-la e retornar a intenção correta para o Bot para atuar. Na próxima seção, adicionaremos lógica ao bot para lidar com essas intenções.

## Modifique o código do bot ##

O Serviço Bot está configurado para funcionar em um ambiente de desenvolvimento tradicional; sincronize seu código fonte com o GIT e trabalhe no seu ambiente de dev. favor. Dito isto, o Azure Bot Service também oferece a capacidade de editar diretamente no portal; o que é ótimo para o nosso experimento. Clique em **Build** e clique em **Open online code editor**.

![img](images/04.png)

No editor de código, abra `BasicLuisDialog.cs`. Ele contém o seguinte código para manipular intenções do aplicativo LUIS.

```cs
using System;
using System.Configuration;
using System.Threading.Tasks;

using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Luis;
using Microsoft.Bot.Builder.Luis.Models;

namespace Microsoft.Bot.Sample.LuisBot
{
    // For more information about this template visit http://aka.ms/azurebots-csharp-luis
    [Serializable]
    public class BasicLuisDialog : LuisDialog<object>
    {
        public BasicLuisDialog() : base(new LuisService(new LuisModelAttribute(
            ConfigurationManager.AppSettings["LuisAppId"], 
            ConfigurationManager.AppSettings["LuisAPIKey"], 
            domain: ConfigurationManager.AppSettings["LuisAPIHostName"])))
        {
        }

        [LuisIntent("None")]
        public async Task NoneIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        // Go to https://luis.ai and create a new intent, then train/publish your luis app.
        // Finally replace "Greeting" with the name of your newly created intent in the following handler
        [LuisIntent("Greeting")]
        public async Task GreetingIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        [LuisIntent("Cancel")]
        public async Task CancelIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        [LuisIntent("Help")]
        public async Task HelpIntent(IDialogContext context, LuisResult result)
        {
            await this.ShowLuisResult(context, result);
        }

        private async Task ShowLuisResult(IDialogContext context, LuisResult result) 
        {
            await context.PostAsync($"You have reached {result.Intents[0].Intent}. You said: {result.Query}");
            context.Wait(MessageReceived);
        }
    }
}
```

## Crie uma classe para armazenar as notas (Notes) ##

No editor de código, abra `BasicLuisDialog.cs`. Ele contém o código para lidar com as intenções Cancel, Greeting, Help e None no Luis.

Adicione a seguinte declaração no inicio do arquivo:

```using System.Collections.Generic;```

Adicione o seguinte código após o construtor BasicLuisDialog:

```cs

        // Store notes in a dictionary that uses the title as a key
        private readonly Dictionary<string, Note> noteByTitle = new Dictionary<string, Note>();

        [Serializable]
        public sealed class Note : IEquatable<Note>
        {

            public string Title { get; set; }
            public string Text { get; set; }

            public override string ToString()
            {
                return $"[{this.Title} : {this.Text}]";
            }

            public bool Equals(Note other)
            {
                return other != null
                    && this.Text == other.Text
                    && this.Title == other.Title;
            }

            public override bool Equals(object other)
            {
                return Equals(other as Note);
            }

            public override int GetHashCode()
            {
                return this.Title.GetHashCode();
            }
        }

        // CONSTANTS
        // Name of note title entity
        public const string Entity_Note_Title = "Note.Title";
        // Default note title
        public const string DefaultNoteTitle = "default";

```

## Manipular a intenção Note.Create ##

Para manipular a intenção Note.Create, adicione o seguinte código à classe `BasicLuisDialog`

```cs

        private Note noteToCreate;
        private string currentTitle;
        [LuisIntent("Note.Create")]
        public Task NoteCreateIntent(IDialogContext context, LuisResult result)
        {
            EntityRecommendation title;
            if (!result.TryFindEntity(Entity_Note_Title, out title))
            {
                // Prompt the user for a note title
                PromptDialog.Text(context, After_TitlePrompt, "What is the title of the note you want to create?");
            }
            else
            {
                var note = new Note() { Title = title.Entity };
                noteToCreate = this.noteByTitle[note.Title] = note;

                // Prompt the user for what they want to say in the note
                PromptDialog.Text(context, After_TextPrompt, "What do you want to say in your note?");
            }

            return Task.CompletedTask;
        }

        private async Task After_TitlePrompt(IDialogContext context, IAwaitable<string> result)
        {
            EntityRecommendation title;
            // Set the title (used for creation, deletion, and reading)
            currentTitle = await result;
            if (currentTitle != null)
            {
                title = new EntityRecommendation(type: Entity_Note_Title) { Entity = currentTitle };
            }
            else
            {
                // Use the default note title
                title = new EntityRecommendation(type: Entity_Note_Title) { Entity = DefaultNoteTitle };
            }

            // Create a new note object 
            var note = new Note() { Title = title.Entity };
            // Add the new note to the list of notes and also save it in order to add text to it later
            noteToCreate = this.noteByTitle[note.Title] = note;

            // Prompt the user for what they want to say in the note
            PromptDialog.Text(context, After_TextPrompt, "What do you want to say in your note?");

        }

        private async Task After_TextPrompt(IDialogContext context, IAwaitable<string> result)
        {
            // Set the text of the note
            noteToCreate.Text = await result;

            await context.PostAsync($"Created note **{this.noteToCreate.Title}** that says \"{this.noteToCreate.Text}\".");

            context.Wait(MessageReceived);
        }

```

## Manipular a intenção Note.ReadAloud ##

O bot pode usar a intenção Note.ReadAloud para mostrar o conteúdo de uma nota ou de todas as notas se o título da nota não for detectado no texto digitado. Cole o seguinte código na classe `BasicLuisDialog` para obter essa funcionalidade.

```cs

        [LuisIntent("Note.ReadAloud")]
        public async Task NoteReadAloudIntent(IDialogContext context, LuisResult result)
        {
            Note note;
            if (TryFindNote(result, out note))
            {
                await context.PostAsync($"**{note.Title}**: {note.Text}.");
            }
            else
            {
                // Print out all the notes if no specific note name was detected
                string NoteList = "Here's the list of all notes: \n\n";
                foreach (KeyValuePair<string, Note> entry in noteByTitle)
                {
                    Note noteInList = entry.Value;
                    NoteList += $"**{noteInList.Title}**: {noteInList.Text}.\n\n";
                }
                await context.PostAsync(NoteList);
            }

            context.Wait(MessageReceived);
        }

        public bool TryFindNote(string noteTitle, out Note note)
        {
            bool foundNote = this.noteByTitle.TryGetValue(noteTitle, out note); // TryGetValue returns false if no match is found.
            return foundNote;
        }

        public bool TryFindNote(LuisResult result, out Note note)
        {
            note = null;

            string titleToFind;

            EntityRecommendation title;
            if (result.TryFindEntity(Entity_Note_Title, out title))
            {
                titleToFind = title.Entity;
            }
            else
            {
                titleToFind = DefaultNoteTitle;
            }

            return this.noteByTitle.TryGetValue(titleToFind, out note); // TryGetValue returns false if no match is found.
        }

```

## Gerenciar a intenção Note.Delete ##

Para a funcionalidade de exclusão de notas, cole o seguinte código na classe `BasicLuisDialog`.

```cs

        [LuisIntent("Note.Delete")]
        public async Task NoteDeleteIntent(IDialogContext context, LuisResult result)
        {
            Note note;
            if (TryFindNote(result, out note))
            {
                this.noteByTitle.Remove(note.Title);
                await context.PostAsync($"Note {note.Title} deleted");
            }
            else
            {
                // Prompt the user for a note title
                PromptDialog.Text(context, After_DeleteTitlePrompt, "What is the title of the note you want to delete?");
            }
        }

        private async Task After_DeleteTitlePrompt(IDialogContext context, IAwaitable<string> result)
        {
            Note note;
            string titleToDelete = await result;
            bool foundNote = this.noteByTitle.TryGetValue(titleToDelete, out note);

            if (foundNote)
            {
                this.noteByTitle.Remove(note.Title);
                await context.PostAsync($"Note {note.Title} deleted");
            }
            else
            {
                await context.PostAsync($"Did not find note named {titleToDelete}.");
            }

            context.Wait(MessageReceived);
        }

```

## Construa o bot ##

Agora que a parte copiar e colar foi finalizada, você pode clicar com o botão direito do mouse em `build.cmd` no editor de código e escolher **Executar no Console**. Seu bot será construído e implantado a partir do ambiente de editor de código on-line.

## Teste o bot ##

No Portal Azure, clique em Testar **no Chat da Web**  para testar o bot. Tente digitar mensagens como &quot;Create note&quot;, &quot;Read my notes&quot; e &quot;Delete notes&quot;. Como você está usando linguagem natural, você tem mais flexibilidade sobre como você declara seu pedido e, por sua vez, o recurso Active Learning da LUIS pode ser usado de tal forma que você pode abrir seu aplicativo LUIS e pode fazer sugestões sobre coisas que você disse, e pode tornar seu aplicativo mais eficaz.

![img](images/05.png)

Dica: se você achar que seu bot nem sempre reconhece a intenção ou as entidades corretas, melhore o desempenho do seu aplicativo LUIS, dando mais exemplos de enunciados para treiná-lo. Você pode alterar seu aplicativo LUIS sem qualquer modificação no código do seu bot.

## Tudo pronto (por hora) ##

Com isso você está apenas começando. Você pode voltar ao serviço de bot e conectar seu bot a vários canais de conversação. Você pode remover as intenções pré-construídas e começar a criar suas próprias intenções personalizadas para sua aplicação.

Há um mundo para descobrir na criação de aplicativos de conversação e é fácil começar. Estamos ansiosos para ver o que você criou e seus comentários. Para mais informações, visite os seguintes sites:

- Serviço Azure Bot: [https://azure.microsoft.com/en-us/services/bot-service/](https://azure.microsoft.com/en-us/services/bot-service/)
- Serviço de compreensão da linguagem (LUIS): [luis.ai](https://www.luis.ai/)
- Arquiteturas de referência, consulte [https://aka.ms/scenarios-abs](https://aka.ms/scenarios-abs)
- Anunciando a disponibilidade geral do Azure Bot Service e LUIS - [https://channel9.msdn.com/Shows/AI-Show/Announcing-General-Availability-of-Azure-Bot-Service-and-Language-Understanding-service](https://channel9.msdn.com/Shows/AI-Show/Announcing-General-Availability-of-Azure-Bot-Service-and-Language-Understanding-service)
