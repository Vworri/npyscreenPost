# Npyscreen Example code

##Figuring out Npyscreen
I have been working on a project for a while now that has a TUI element. Because of this, I have had to get to know Npyscreen. I have also realized that there is not much about building an app and adding functionality to it. So I decided to write about what I have figured out (since the last time I wrote about Npyscreen).


---

##Installing Npyscreen
First, to install Npyscreen, you can use pip:
```pip install npyscreen```
Conda:
```conda install npyscreen```
##Creating the initial TUI
I have used the npyscreen.NPSAppManaged  class because it allows me to organise my different forms. The class is initialised by the onStart(self) method.

<script src="https://gist.github.com/Vworri/63701cb4bc8618efdb6a581c8450b108.js"></script>


In onStar(self): , I have initialized my first form (which I have not created yet). The main form has to have the name MAIN. I have also defined our main() function to run the NPSAppManaged class we have created. 
Now we can go ahead and create our myTUI form.
<script src="https://gist.github.com/Vworri/7c7d6dde561fa6b4f1e8c294e65faac3.js"></script>


myTui is an Npyscreen.Form class. In that class, we need to define a create(self) method in which we define the different elements/widgets that our form will house. We append the different widgets to the form by using self.add which takes a few arguments: 
type (the npyscreen.TitleFixedText) this is required
name is not required but it is the label for the widget
value is what the text field will say initially

You do not have to assign each widget to a variable but you should if you want to be able to manipulate the widget outside the create function.
When you run the code (in a terminal), you end up with this:
![](https://cdn-images-1.medium.com/max/1600/1*fhbnE8-sLRxCvmkXq-RqHw.png)
Now, I want Dave to be able to respond so I will add another widget in which the user can input text.
<script src="https://gist.github.com/Vworri/6e44afaf868b2becb5fd09c0ee272500.js"></script>

![](https://cdn-images-1.medium.com/max/1600/1*yUw6FNOKCtqf7Ty8E-4lvg.png)

**NB: If you haven’t seen/read 2001 a Space Odyssey, you won’t get any of these jokes**


I can go in and change the text in the new widget.
Now, I will go ahead and create a questionnaire so we can evaluate Hal’s performance. We can do this by adding more widgets. 
<script src="https://gist.github.com/Vworri/5aa21b81bca28c8aa7bda582fbfa51d7.js"></script>


There are many widget options with different parameters. There are some good examples above but the docs are nicely written on this matter. When rendered, the TUI looks and works like this:
![](https://cdn-images-1.medium.com/max/1600/1*fMWOxOvTDzO1GCV77VmSWQ.png)
##Form Methods
Forms have useful methods of their own. The ones I have used are:

- create(self): We have already seen this. It gets called only once when the form is initialized.
- while_editing(self): This gets called whenever the user strikes a key
- after_editing(self): This gets fired when self.editing is changed to False like when you press ok, or if you manually change it somewhere
- on_ok(self): This gets triggered when you press the ok button. By default, it turns editing to false.
- activate(self): This gets triggered when the form is activated. It can be used to initialize certain functions.

##Creating the next form
Now that we have our main form, we can create another one. This one will return the input given in the myTUI so that Hal can see what Dave actually thinks about him. 

<script src="https://gist.github.com/Vworri/ee771e49702dcee1a55c8c40dd7eee79.js"></script>

The next step is to add it to onStart(self) in thenpyscreen.NPSAppManaged class we created.

<script src="https://gist.github.com/Vworri/ec3f2108809b8d5a79bc3d9fe2925529.js"></script


If we ran the TUI as is, we will not see this form. We need to go back to the previous form class (myTUI i.e. MAIN) and tell it about the new one.
In the myTUI class, we should add an activate(self) method this method. Before we do this, we need to understand a bit about how npyscreen.NPSAppManaged works:

![](https://cdn-images-1.medium.com/max/1600/1*ZFvrLp7Tm71KLqfeZhEOzQ.png)

The two forms are housed inside the parent app: our npyscreen.NPSAppManaged class. This means, to interact with other forms from a form class, we must first go through the parent app. In activate, we must first tell npyscreen.NPSAppManaged that we want to edit this app, then, when we press ok, switch to “Hal’s”  form.

<script src="https://gist.github.com/Vworri/81bdbcd33a3ddb73e79a4c3dca286cf0.js"></script>

## Getting User input and Using it
This is where I got stuck when building my project. The documentation has no real information on how to pass user input information from one form to another. 
You can extract the value of the widget locally by using:

<script src="https://gist.github.com/Vworri/2f08653ef824fe87f355183370ca2c11.js"></script>

Some widgets will return a string, others will return an array depending.
The trick is to assign the value of a widget in one form to the value of a widget in another. 
To do this, we need to call the next form through the parent app. Then we can access the widgets.
I will do this under the on_ok(self) method inside the myTui class.

<script src="https://gist.github.com/Vworri/74624dd260c354f8a328a71db2952b73.js"></script>



Notice that we can use the dot notation to return any attribute of a widget.

Now, I can fill out the form:
![](https://cdn-images-1.medium.com/max/1600/1*EOBhi9SXOmLebKwrKWVt7A.png)

And pass the input:
![](https://cdn-images-1.medium.com/max/1600/1*1kt9NClbaUxTet94XwCeqw.png)

Woooooot. So far, I haven’t found a more elegant way of passing on variables. 
If they are constant, and you want to define them somewhere, define them in the parent app, before the onStart(self) method.

<script src="https://gist.github.com/Vworri/f414fb9bf9e933a3a249ea4d10c6c531.js"></script>

and then access it using self.parentApp.myVariable in any of the form classes.
<script src="https://gist.github.com/Vworri/2f08653ef824fe87f355183370ca2c11.js"></script>

If you want to update the value of myVariable, you will have problems. I have not been able to figure it out so I made a work-around.
I passed the values into the new Form with the on_ok(self) method: just as above. But I changed the editable attribute from the default: True to False . If I needed the value to feed into a function and not display it, you can add hidden=True. Yo can still access the value with:



Now, the entire code looks like this:

<script src="https://gist.github.com/Vworri/bef331a79d56112ddcb25833eb9286a8.js"></script>

