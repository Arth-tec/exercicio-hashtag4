# tela inicial
# titulo
# botao iniciar
# quando clicar no botao
# abrir um alerta: abrir um popapi
# titulo: bem vindo
# caixa de texto: escrever seu nome
# botao: entrar no chat
# quando clicar no botao
# sumir com o titulo
# sumir com o botao iniciar chat
# carregar o chat
# carregar o campo de enviar mensagem: digite sua mensagem
# botao enviar
# quando clicar no botao enviar
# enviar a mensagem
# limpar a caixa de mensagem

# flet
# importar o flet
import flet as ft

# criar uma funçao principal para rodar o seu aplicativo


def main(pagina):
    # titulo
    titulo = ft.Text("Chat")
    pagina.add(titulo)

    def enviar_mensagem_tunel(mensagem):
        texto = ft.text(mensagem)
        chat.controls.append(texto)
        pagina.update()

    pagina.pubsub.subscribe(enviar_mensagem_tunel)

    def enviar_mensagem(evento):
        nome_usuario = caixa_nome.value
        texto_campo_mensagem = campo_enviar_mensagem.value
        mensagem = f"{nome_usuario}: {texto_campo_mensagem}"
        pagina.pubsub.send_all(mensagem)

        campo_enviar_mensagem.value = ""
        texto = ft.Text(f"{nome_usuario}: {texto_campo_mensagem}")
        chat.controls.append(texto)
        pagina.update()

    campo_enviar_mensagem = ft.TextField(label="Digite aqui sua mensagem")
    botao_enviar = ft.ElevatedButton("Enviar", on_click=enviar_mensagem)
    linha_enviar = ft.Row([campo_enviar_mensagem, botao_enviar])

    chat = ft.Column()

    def entrar_chat(evento):
        popup.open = False
        pagina.remove(titulo)
        pagina.remove(botao)
        pagina.add(chat)
        pagina.add(linha_enviar)

        nome_usuario = caixa_nome.value
        texto_mensagem = ft.Text(f"{nome_usuario} entrou no chat")
        chat.controls.append(texto_mensagem)

        pagina.update()

    # criar o popup
    titulo_popup = ft.Text("Bem vindo ao chat")
    caixa_nome = ft.TextField(label="Digite seu nome")
    botao_popup = ft.ElevatedButton("Entrar no chat", on_click=entrar_chat)

    popup = ft.AlertDialog(title=titulo_popup, content=caixa_nome,
                           actions=[botao_popup])

    # botao inicial

    def abrir_popup(evento):
        pagina.dialog = popup
        popup.open = True
        pagina.update()

    botao = ft.ElevatedButton("Iniciar Chat", on_click=abrir_popup)
    pagina.add(botao)


# executar essa funçao com o flet
ft.app(target=main, view=ft.WEB_BROWSER, port=8000)

