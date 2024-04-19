import speech_recognition as sr
import pyttsx3
import pygame

detectar = sr.Recognizer()
maquina = pyttsx3.init()
maquina.say('Olá, sou a Luna.')
maquina.say('Como posso ajudá-lo?')
maquina.runAndWait()

musicas = ['musica1.mp3', 'musica2.mp3', 'musica3.mp3']
indice_musica_atual = 0

def tocarMusica():
    pygame.init()  
    pygame.mixer.music.load(musicas[indice_musica_atual])
    pygame.mixer.music.play()
    while pygame.mixer.music.get_busy():
        pygame.time.Clock().tick(20)
        executar_comando()    

def pausarMusica():
    pygame.mixer.music.pause()
    executar_comando()

def retomarMusica():
    pygame.mixer.music.unpause()
    executar_comando()

def proximaMusica():
    global indice_musica_atual
    indice_musica_atual = (indice_musica_atual + 1) % len(musicas)
    tocarMusica()

def executar_comando():
    with sr.Microphone() as source:
        print("ola, estou ouvindo")
        try:
            audio = detectar.listen(source)
            comando = detectar.recognize_google(audio, language='pt-BR')
            print("Você disse:"+comando)
            if 'tocar' in comando:
                tocarMusica()
            elif 'pausar' in comando:
                pausarMusica()
            elif 'retomar' in comando:
                retomarMusica()
            elif 'próxima' in comando:
                proximaMusica()
        except sr.UnknownValueError:
            maquina.say("Eu não entendi o que você disse")
        
executar_comando()
