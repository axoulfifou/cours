# Doc code caméra-QR code

Il y a besson que de 2 librairies

- opencv

code pour installer: pip install opencv-python

ou

(cela dépend de la version de ton pip) :pip3 install opencv-python

- tkinter

code pour installer

pip install tk

pip3 install tk

si cela ne fonctionne pas essaye avec la connexion de ton téléphone

recettte_scanqr.py est le programme de base

lib_scanqr.py est  une fonction dont on fait appel dans scanqr

si tu veux arrêter la caméra quand le script fonctionne il te suffit juste de déplacer ta souris sur la fenêtre de la caméra puis d'appuyer sur Q

[le programme en un fichier](https://docs.google.com/document/d/1mBTkdhXaKx9JRsPEZaz8OQzHvxnv_3iFcekgbTZRuUI/edit?usp=sharing) =

from tkinter import *

from tkinter.ttk import *

import cv2

master = Tk()

master.geometry("600x550")

port_usb=0

CodeQr = "Blop"

#allume en vert

def allume_vert():

canvas.create_oval(20,20,80,80, fill="green")

canvas.pack()

#allume en rouge

def allume_red():

canvas.create_oval(20,140,80,80, fill="red")

canvas.pack()

#reset les voyant

def init_led():

canvas.create_oval(20,20,80,80, fill="white")

canvas.create_oval(20,140,80,80, fill="white")

canvas.pack()

# Fonction permettant de scanner des Qrcodes et de retourner le Qrcode scanné.

# PE : Numéro du port usb

# PS : Le QrCode scanné ou "ERREUR" si arrêt de l'utilisateur "Touche q"

def scanqr(port_usb):

Code = "ERREUR"

vid = cv2.VideoCapture(port_usb)

detector = cv2.QRCodeDetector()

while True:

ret, frame = vid.read()

data, bbox, straight_qrcode = detector.detectAndDecode(frame)

cv2.imshow('frame', frame)

if len(data) > 0:

Code=data

break

if cv2.waitKey(1) & 0xFF == ord('q'):

break

vid.release()

cv2.destroyAllWindows()

return Code

#pas utiliser

def test_window():

newWindow = Toplevel(master)

newWindow.title("ScanQRcode")

newWindow.geometry("200x200")

# Fonction permettant de scanner des Qrcodes et de retourner le Qrcode scanné.

# PE : Numéro du port usb

# PS : Le QrCode scanné ou "ERREUR" si arrêt de l'utilisateur "Touche q"

def appel_scanqr():

init_led()

CodeQr = scanqr(port_usb)

canvas.create_rectangle(380,400,200,200,width=0,fill='white')

if (CodeQr) == "LACHAUDLOUCAS" :

canvas.create_text(300,230, text= CodeQr)

allume_vert()

else:

canvas.create_text(300,230, text= CodeQr)

allume_red()

canvas = Canvas(width=800, height=800, bg='white')

canvas.create_oval(20,20,80,80, fill="white")

canvas.create_oval(20,140,80,80, fill="white")

canvas.pack()

#bouton

btn = Button(canvas,

text ="Click pour scanner",

command = appel_scanqr)

btn.pack(side=TOP, padx=250, pady=250)

# tjrs a la fin

mainloop()

sur raspberry

il faut utiliser la commande sudo apt install python3-opencv

voici la docs [Raspberry docs](https://raspberrytips.fr/installer-opencv-sur-raspberry-pi/) : https://raspberrytips.fr/installer-opencv-sur-raspberry-pi/