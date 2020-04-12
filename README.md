# Medical-Record-System
from tkinter import *
import sqlite3

def find1(): # personal information tab
    
    try:
    
        name = eName.get()        
        age = eAge.get()
        ph_no = ePh_No.get()
        blood = eBlood_Group.get()
        gender = ""
        if(v.get()==1):
            gender = "male"
        else:
            gender = "female"
        
        if(len(name) == 0):
            msg = "Enter valid name"
            popupmsg(msg)
        elif(len(age) == 0):
            msg = "Enter valid age"
            popupmsg(msg)
        elif(int(age)<0 or int(age)>115):
            msg = "Enter valid age"
            popupmsg(msg)
        elif(len(blood)<10):
            msg = "Enter valid Blood Group "
            popupmsg(msg)
        elif(len(ph_no) != 10):
            msg = "Enter valid 10-digit phone number"
            popupmsg(msg)
        else:
            print(name, age, blood, ph_no, gender)

    except:
        pass
    
    
    def find2(): # height and weight details to get BMI index and get tips acc to BMI value, then bifurcate on basis of Blood group.
        feet = int(feet_get.get())
        inch = int(inch_get.get())
        #print(feet, inch)
        height = (feet*30+inch)/100
        #print(height)
        weight = eWeight.get()
        bmi = calculate(height, weight)
        
        try:
            second.destroy()
        except:
            pass
      
        def save():
            connection = sqlite3.connect("myTable.db")
            cursor = connection.cursor()
        
    #sql_command = """ DROP TABLE TABLE IF EXISTS patient; """
    #cursor.execute(sql_command)
            try:
                sql_command = """ create TABLE patient2(name VARCHAR(30), age INTEGER, 
                blood VARCHAR(15), gender VARCHAR(6), bmi INTEGER);"""
                cursor.execute(sql_command)
            except:
                pass
            sql_command = "INSERT INTO patient2(name, age, blood, gender, bmi) VALUES('{}',{},'{}','{}',{});".format(name, age, blood, gender, bmi)
            print(sql_command)
            cursor.execute(sql_command)
            cursor.execute("SELECT * FROM patient2")
            ans=cursor.fetchall()
    
            #for i in ans:
             #   print(i)
        
            connection.commit()
            connection.close()
            
        if(bmi<18.5):
            msg="You are underweight. Eat more protein"
            pop_bmi(msg)
        elif(bmi>18.5 and bmi<25):
            msg="You are normal. Keep up the good work to maintain your current weight"
            pop_bmi(msg)
        elif(bmi>25 and bmi<30):
            msg="You are overweight. Try to reduce your weight"
            pop_bmi(msg)
        else:
            msg="You are obese. Follow a diet to overcome your situation"
            pop_bmi(msg)
        
        BMI = Tk()
        Label(BMI, text="Your BMI is " + str(bmi)).grid(row=0)
        Label(BMI, text="").grid(row=1)
        Label(BMI, text="").grid(row=2)
        if(bmi<18.5):
            Label(BMI, text="Below 18.5 is Underweight", fg="red").grid(row=3)
            Label(BMI, text="Between 18.5 and 25 is Normal").grid(row=4)
            Label(BMI, text="Between 25 and 30 is Overweight").grid(row=5)
            Label(BMI, text="More than 30 is Obese").grid(row=6)
            
        elif(bmi>18.5 and bmi<25):
            Label(BMI, text="Below 18.5 is Underweight").grid(row=3)
            Label(BMI, text="Between 18.5 and 25 is Normal", fg="red").grid(row=4)
            Label(BMI, text="Between 25 and 30 is Overweight").grid(row=5)
            Label(BMI, text="More than 30 is Obese").grid(row=6)
            
        elif(bmi>25 and bmi<30):
            Label(BMI, text="Below 18.5 is Underweight").grid(row=3)
            Label(BMI, text="Between 18.5 and 25 is Normal").grid(row=4)
            Label(BMI, text="Between 25 and 30 is Overweight", fg="red").grid(row=5)
            Label(BMI, text="More than 30 is Obese").grid(row=6)
            
        else:
            Label(BMI, text="Below 18.5 is Underweight").grid(row=3)
            Label(BMI, text="Between 18.5 and 25 is Normal").grid(row=4)
            Label(BMI, text="Between 25 and 30 is Overweight").grid(row=5)
            Label(BMI, text="More than 30 is Obese", fg="red").grid(row=6)
        
        Label(BMI, text="").grid(row=7)
        Label(BMI, text="").grid(row=8)

        btnSave = Button(BMI, text="Save", command=save, width=25) #add command later
        btnSave.grid(row=9)
        Label(BMI, text="").grid(row=10)
        btnBack = Button(BMI, text="Back", command=find1, width=25)
        btnBack.grid(row=11)
        Label(BMI, text="").grid(row=12)
        btnView = Button(BMI, text="View History", command=show, width=25)
        btnView.grid(row=13)
        BMI.mainloop()
   
    try:
        main.destroy()
    except:
        pass
    
    try:
        show.destroy()
    except:
        pass
    
    try:
        BMI.destroy()
    except:
        pass
    
    try:
        show.destroy()
    except:
        pass

    def show():
    
        try:
            second.destroy()
        except:
            pass
        
        try:
            BMI.destroy()
        except:
            pass
    
        show = Tk()
        Label(show, text="Saved Medical Records").pack()
        Label(show, text="").pack()
        lb = Listbox(show)
        lb.pack()
        scrollH = Scrollbar(show, orient="horizontal")
        scrollV = Scrollbar(show, orient="vertical")
        scrollH.config(command=lb.xview)
        scrollV.config(command=lb.yview)
        scrollH.pack(side="bottom", fill="x")
        scrollV.pack(side="right", fill="y")
        lb.config(xscrollcommand=scrollH.set)
        lb.config(yscrollcommand=scrollV.set)
        connection = sqlite3.connect("myTable.db")
        cursor = connection.cursor()
        lb.insert(1,"Blood Group: O positive")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'O positive'")
        ans=cursor.fetchall()
        a=2
        for i in ans:
            lb.insert(a,i)
            a=a+1
      
        lb.insert(a,"")
        a=a+1
        lb.insert(a,"Blood Group: A positive")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'A positive'")
        ans=cursor.fetchall()
        a=a+1
        for i in ans:
            lb.insert(a,i)
            a=a+1
        
        lb.insert(a,"")
        a=a+1
        lb.insert(a,"Blood Group: B positive")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'B positive'")
        ans=cursor.fetchall()
        a=a+1
        for i in ans:
            lb.insert(a,i)
            a=a+1
        
        lb.insert(a,"")
        a=a+1
        lb.insert(a,"Blood Group: AB positive")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'AB positive'")
        ans=cursor.fetchall()
        a=a+1
        for i in ans:
            lb.insert(a,i)
            a=a+1
            
        lb.insert(a,"")
        a=a+1
        lb.insert(a,"Blood Group: O negative")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'O negative'")
        ans=cursor.fetchall()
        a=a+1
        for i in ans:
            lb.insert(a,i)
            a=a+1
            
        lb.insert(a,"")
        a=a+1
        lb.insert(a,"Blood Group: A negative")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'A negative'")
        ans=cursor.fetchall()
        a=a+1
        for i in ans:
            lb.insert(a,i)
            a=a+1
            
        lb.insert(a,"")
        a=a+1
        lb.insert(a,"Blood Group: B negative")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'B negative'")
        ans=cursor.fetchall()
        a=a+1
        for i in ans:
            lb.insert(a,i)
            a=a+1
            
        lb.insert(a,"")
        a=a+1
        lb.insert(a,"Blood Group: AB negative")
        cursor.execute("SELECT * FROM patient2 WHERE blood = 'AB negative'")
        ans=cursor.fetchall()
        a=a+1
        for i in ans:
            lb.insert(a,i)
            a=a+1
        
        btnBack = Button(show, text="Back", command=find1, width=25)
        btnBack.pack(side="bottom")
        show.mainloop()

    
    second = Tk()
    Welcome = Label(second, text="Welcome ")
    Welcome.grid(row=0)
    Height = Label(second, text="Height")
    Height.grid(row=1)
    Feet = Label(second, text="Feet")
    Feet.grid(row=2)
    feet_get = Spinbox(second, from_= 1, to = 11)
    feet_get.grid(row=3)
    Inch = Label(second, text="Inch")
    Inch.grid(row=4)
    inch_get = Spinbox(second, from_=1, to = 11)
    inch_get.grid(row=5)
    Weight = Label(second, text="Weight")
    Weight.grid(row=6)
    eWeight = Entry(second) #, cursor="center")
    eWeight.grid(row=7, column=0)
    Label(second, text="").grid(row=8)
    Label(second, text="").grid(row=10)
    btnView_History = Button(second, text="View History", width=25, command=show) # add a command later
    btnView_History.grid(row=11)
    btnCalculate = Button(second, text="Calculate", width=25, command=find2) # add a command later
    btnCalculate.grid(row=9)
    second.mainloop()
    
    
def calculate(height, weight):
    height1 = float(height)
    weight1 = float(weight)
    bmi1 = weight1/(height1*height1)
    bmi = round(bmi1,4)
    print("bmi "+str(bmi))
    return(bmi) 


def popupmsg(msg):
    
    def leave():
        popup.destroy()
        
    popup = Tk()
    popup.wm_title("!")
    Label(popup, text = msg).pack(side="top", fill="x", pady=10)
    Button(popup, text="Okay", command=leave).pack()
    popup.mainloop()
    
def pop_bmi(msg):
    
    def leave1():
        underweight.destroy()
    
    underweight = Tk()
    underweight.wm_title("!")
    Label(underweight, text=msg).pack(side="top", fill="x", pady=10)
    Button(underweight, text="Okay", command=leave1).pack()
    underweight.mainloop()

main = Tk()
personal_details = Label(main, text="Personal Details")
personal_details.grid(row=0)

Label(main, text="Name:").grid(row=1)
Label(main, text="Age:").grid(row=2)
Label(main, text="Blood type eg: O positive").grid(row=3)
Label(main, text="Phone Number:").grid(row=4)
eName = Entry(main)
eAge = Entry(main)
ePh_No = Entry(main)
eBlood_Group = Entry(main)
eName.grid(row=1, column=1)
eAge.grid(row=2, column=1)
eBlood_Group.grid(row=3, column=1)
ePh_No.grid(row=4, column=1)
v = IntVar()
Radiobutton(main, text="Male", variable=v, value=1).grid(row=5)
Radiobutton(main, text="Female", variable=v, value=2).grid(row=6)
Label(main, text="").grid(row=7)
btnRegister = Button(main, text="Register", width=20, command=find1) # add a command later
btnRegister.grid(row=8)
main.mainloop()
