# primos
Pri
def pri(n):
  Primos=[]
  no_primos=[]
  for i in range(2,n+1):
    if i not in no_primos:
      primos.append(i)
    for j in range(i*i,n+1,i):
      no_primos.append(j)
  return primos

 
import tkinter as tk
from tkinter.messagebox import *
import mysql.connector as mysql

mycon = mysql.connect(
    user='your username here',
    passwd='your password here',
    host='localhost',
    database='to_do'
)

def add_to_db(task):
    mycur = mycon.cursor()
    mycur.execute('INSERT into tasks(task) values("{}")'.format(task))
    mycon.commit()

def delete_from_db(task):
    mycur = mycon.cursor()
    mycur.execute('DELETE from tasks where task = "{}"'.format(task))
    mycon.commit()

def add_task():
    task = task_entry.get()
    if task!= '':
        tasks_list.insert(tk.END,task)
        add_to_db(task)
        task_entry.delete(0,tk.END)
    else:
        showerror('Error','Please Enter A Task')

def del_task():
    try:
        selection = tasks_list.curselection()[0]
        task = tasks_list.get(selection)
        delete_from_db(task)
        tasks_list.delete(selection)
    except:
        showerror('Error','Please Select a Task to Delete')

def load_task():
    tasks_list.delete(0,tk.END)
    mycur = mycon.cursor()
    mycur.execute('select * from tasks')
    tasks = mycur.fetchall()
    if len(tasks) == 0:
        showerror('No tasks!','No tasks are saved!')
    for task in tasks:
        tasks_list.insert(tk.END,task[0])


root = tk.Tk()
root.config(bg='#ede3d9')
root.title('To-Do App @load_thecode')
root.geometry('290x420+550+250')
root.iconbitmap('icon.ico')

bg_picture = tk.PhotoImage(file='todo_bg.png')
bg = tk.Label(root,image=bg_picture)
bg.pack()

tasks_list = tk.Listbox(root,width=50,font=('Times', 15),bg='#ede3d9',fg='#000000')
tasks_list.pack()


task_entry = tk.Entry(root,width=50,font=('Times',15),bg='#ede3d9',fg='#000000')
task_entry.pack()


add_task_button = tk.Button(root,text='Add Task',command=add_task,width=45,font=('Helvetica',10),bg='#ede3d9',fg='#000000',relief=tk.FLAT)
add_task_button.pack()

delete_task_button = tk.Button(root,text='Delete Task',command=del_task,width=45,font=('Helvetica',10),bg='#ede3d9',fg='#000000',relief=tk.FLAT)
delete_task_button.pack()


clear_task_button = tk.Button(root,text='Load Tasks',command=load_task,width=45,font=('Helvetica',10),bg='#ede3d9',fg='#000000',relief=tk.FLAT)
clear_task_button.pack()



root.mainloop()
