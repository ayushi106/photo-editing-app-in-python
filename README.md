# move an Image on the canvas with tkinter
from tkinter import *
from tkinter import filedialog
from tkinter.filedialog import askopenfilename
import tkinter.colorchooser
from tkinter import messagebox
from PIL import ImageTk,Image
from PIL import ImageGrab
import tkinter as tk
from PIL import ImageFilter
from PIL import Image
from PIL import ImageEnhance



# Create the window with the Tk class
root = Tk()

root.title("Picture Collage")
root.geometry("1000x800")



# Create the canvas and make it visible with pack()
canvas = tk.Canvas(root,width=800,height=800,bg = "light green")
canvas.pack(fill=BOTH) # this makes it visible


var=StringVar()
var.set(" WELCOME TO PICTIÚR \n\n\n YOUR VERY OWN PICTURE EDITOR \n\n AND COLLAGE MAKER \n\n\n\n Created By\n\n Mohapar Singh & Ayushi Sahu")
label1=Label(root,textvariable=var,bg="pink",anchor=CENTER,width=50,height=20)
label1.pack()
label1.place(x=300,y=200)
root.after(8000 , label1.destroy)

count=0
l=[]
z=[]
a=-1
h=[]
g=[]
#to select an image
def select_image():
	global a
	a = int(e1.get())-1
#for choosing background color 
def bg_color_button():
    color = tk.colorchooser.askcolor()
    canvas.configure(bg=color[1])
#to display the image
def display_image():
	global count
	global f
	global a


	f=tk.filedialog.askopenfilename()
	f = f.replace('/','\\')
	g.append(f)

	h.append(Image.open(f).resize((150,150), Image.ANTIALIAS))
	l.append(ImageTk.PhotoImage(h[count]))
	canvas.image=l[count]
z.append(canvas.create_image(50,50, image=l[count]))
#to move the image around in the canvas	
	def move(event):
		"""Move the sprite image with a d w and s when click them"""
		if event.char == "a":
			canvas.move(z[a], -10, 0)
		elif event.char == "d":
			canvas.move(z[a], 10, 0)
		elif event.char == "w":
			canvas.move(z[a], 0, -10)
		elif event.char == "s":
			canvas.move(z[a], 0, 10)
 
	
	# This bind window to keys so that move is called when you press a key
root.bind("<Key>", move)
count+=1	

a+=1
#for resizing the image
def resize():
	global f
	global a
	
	x = int(e2.get())
	y = int(e3.get())

	h[a]=Image.open(g[a]).resize((x,y), Image.ANTIALIAS)

	l[a]=ImageTk.PhotoImage(h[a])
	canvas.image=l[a]
	z[a]=canvas.create_image(50,50, image=l[a])
#rotate function	
def rotate():
	global f
	global a

	h[a]=h[a].rotate(90)

	l[a]=ImageTk.PhotoImage(h[a])
	canvas.image=l[a]
	z[a]=canvas.create_image(50,50, image=l[a])
#blur function
def blur():
	global f
	global a
	
	h[a]=h[a].filter(ImageFilter.BLUR)
	
	l[a]=ImageTk.PhotoImage(h[a])
	canvas.image=l[a]
	z[a]=canvas.create_image(50,50, image=l[a])
#to turn the image black and white
def b_and_w():
	global f
	global a

	h[a]=h[a].convert('1')

	l[a]=ImageTk.PhotoImage(h[a])
	canvas.image=l[a]
	z[a]=canvas.create_image(50,50, image=l[a])
#delete everything that is on canvas
def delete_all():
	canvas.delete(ALL)
#delete the selected image
def delete_recent():
	global count
	canvas.delete(z[count-1])
#save the editted image
def save():
	filename = e4.get()
	
print("Your image has been saved successfully")
canvas.update()
#	
def getter(widget):
		x=root.winfo_rootx()+widget.winfo_x()
		y=root.winfo_rooty()+widget.winfo_y()
		x1=x+widget.winfo_width()
		y1=y+widget.winfo_height()
		ImageGrab.grab().crop((x,y,x1,y1)).save(filename +".jpg")
	getter(canvas)
	canvas.create_text(1000,10,fill="black",font="Times 10 italic bold",text="Image succesfully saved")
#
def instruct():
	var2=StringVar()
	var2.set("1.Hello user ,to get started choose the backgroung color. \n\n 2.Then upload the image one by one,you can upload many images but for easy access remember the image number order to select an image. \n\n 3.To move the image around use 'a'=left,'w'=down,'d'=left,'w'=up. \n\n 4.Resize the image to your requirement. \n\n 5.REMEMBER EVERY TIME YOU RESIZE YOUR IMAGE ALL OTHER EFFECTS WILL BE WORN OFF RESIZE WISELY,\n To resize enter the parameters and click RESIZE  . \n\n 6.Next click the button to perform other function on image. \n\n 7 .To work with an image input the sequence number of the image eg If you uploaded a second picture then its sequence number is 2. \n\n 8.To save the image enter the file name and the picture will be SAVED WHERE THE APP IS SAVED IN THE LAPTOP. \n\n 9.To delete everything on the screen click DELETE ALL. \n\n 10.To delete the selected picture ,Select the picture and click DELETE RECENT  ")
	label=Label(root,textvariable=var2,bg="light blue",anchor=CENTER,width=110,height=25)
	label.pack()
	label.place(x=70,y=100)
	root.after(10000 , label.destroy)


	
inputvar1=StringVar()
inputvar1.set("")

inputvar2=StringVar()
inputvar2.set("")

inputvar3=StringVar()
inputvar3.set("")

inputvar4=StringVar()
inputvar4.set("")


b12=Button(root,text="Instructions",command=instruct,height=3,width=20)
b12.place(x=900,y=20)

b10=Button(root, text="Select Backround Color", command=bg_color_button)
b10.place(x=900,y=100)

b13=Button(root, text="Upload image", command=display_image)
b13.place(x=900,y=140)

b9=Button(root,text="Resize image",command=resize)
b9.place(x=900,y=180)

b6=Button(root,text="Rotate",command=rotate)
b6.place(x=900,y=220)

e3=Entry(root,width="12",textvariable=inputvar3)
e3.place(x=1000,y=180)

e2=Entry(root,width="12",textvariable=inputvar2)
e2.place(x=1080,y=180)

b11=Button(root,text="Blur",command=blur)
b11.place(x=950,y=220)

b12=Button(root, text='Select Image', command=select_image)
b12.place(x=900,y=260)

e1=Entry(root,width="12",textvariable=inputvar1)
e1.place(x=1000,y=260)

b5=Button(root,text="Save",command=save)
b5.place(x=900,y=300)

e4=Entry(root,width="12",textvariable=inputvar4)
e4.place(x=1000,y=300)

b10=Button(root,text="Delete Recent",command=delete_recent)
b10.place(x=900,y=340)

b3=Button(root,text="Delete all",command=delete_all)
b3.place(x=900,y=380)

b7=Button(root,text="Black&White",command=b_and_w)
b7.place(x=987,y=220)



# this creates the loop that makes the window stay 'active'
mainloop()
