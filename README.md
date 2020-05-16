# VAT-Calculator-GUI
#VAT calculator, TKinter
#Calculate net, vat and total from any one input.


from tkinter import *

# set-up window
window = Tk()
window.geometry('280x290')
window.resizable(0, 0)
window.title('VAT calculator')
window.iconbitmap('Calculator.ico')

# Instructions for user
instruction = Label(window,
                    text='Enter net value in Net field or total \n value in Total field or VAT value \n in VAT field and press Calculate.',
                    padx=10, pady=10)
instruction.grid(column=1, sticky=W)
instruction2 = Label(window, text='Press Clear for new calculation.', padx=5, pady=5)
instruction2.grid(column=1, sticky=W)


# clear function
def clear():
    net.delete(0, 'end')
    vat.delete(0, 'end')
    total.delete(0, 'end')


# calculate from net function
def net_calc(net_value):
    vat_plus_value = float(net_value) / 5
    total_plus_value = float(net_value) + float(vat_plus_value)

    net.delete(0, END)
    net_value_insert = "{: .2f}".format(float(net_value))
    net.insert(0, net_value_insert)

    vat.delete(0, END)
    vat_value_insert = "{: .2f}".format(float(vat_plus_value))
    vat.insert(0, vat_value_insert)

    total.delete(0, END)
    total_value_insert = "{: .2f}".format(float(total_plus_value))
    total.insert(0, total_value_insert)


# back calculate from total function
def total_calc(total_value):
    vat_minus_value = float(total_value) / 6
    net_minus_value = float(vat_minus_value) * 5

    net.delete(0, END)
    net_minus_value_insert = "{: .2f}".format(float(net_minus_value))
    net.insert(0, net_minus_value_insert)

    vat.delete(0, END)
    vat_minus_value_insert = "{: .2f}".format(float(vat_minus_value))
    vat.insert(0, vat_minus_value_insert)

    total.delete(0, END)
    total_minus_value_insert = "{: .2f}".format(float(total_value))
    total.insert(0, total_minus_value_insert)


# calculate net and gross from VAT
def vat_calc(vat_value):
    net_calc_value = float(vat_value) * 5
    total_calc_value = float(vat_value) * 6

    net.delete(0, END)
    net_value_insert = "{: .2f}".format(float(net_calc_value))
    net.insert(0, net_value_insert)

    vat.delete(0, END)
    vat_value_insert = "{: .2f}".format(float(vat_value))
    vat.insert(0, vat_value_insert)

    total.delete(0, END)
    total_value_insert = "{: .2f}".format(float(total_calc_value))
    total.insert(0, total_value_insert)


# calculate add or minus function
# ensure only one field is used
# ensure only numbers entered in fields
def calculate():
    # acceptable character list
    char_ok = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '.']
    net_value = net.get()
    total_value = total.get()
    vat_value = vat.get()

    if len(net_value) > 0 and len(total_value) == 0 and len(vat_value) == 0:

        def split(net_entry):
            return list(net_entry)

        net_entry = str(net_value)
        net_value_split = (split(net_entry))

        check = any(item in char_ok for item in net_value_split)
        if check == True:
            net_calc(net_value)
        else:
            clear()

    elif len(net_value) == 0 and len(total_value) > 0 and len(vat_value) == 0:

        def split(total_entry):
            return list(total_entry)

        total_entry = str(total_value)
        total_value_split = (split(total_entry))

        check = any(item in char_ok for item in total_value_split)
        if check == True:
            total_calc(total_value)
        else:
            clear()

    elif len(net_value) == 0 and len(total_value) == 0 and len(vat_value) > 0:

        def split(vat_entry):
            return list(vat_entry)

        vat_entry = str(vat_value)
        vat_value_split = (split(vat_entry))

        check = any(item in char_ok for item in vat_value_split)
        if check == True:
            vat_calc(vat_value)
        else:
            clear()

    else:
        clear()


# labels
label_1 = Label(window, text='Net', pady=10)
label_1.grid(row=3, sticky=W)

label_2 = Label(window, text='VAT', pady=10)
label_2.grid(row=4, sticky=W)

label_3 = Label(window, text='Total', pady=10)
label_3.grid(row=5, sticky=W)

# buttons
calculate_btn = Button(window, text='Calculate', width=10, bg='black', fg='white', command=calculate, padx=10, pady=5)
calculate_btn.grid(column=1, row=6, sticky=W)

clear_btn = Button(window, text='Clear', width=10, bg='black', fg='white', command=clear, padx=10, pady=5)
clear_btn.grid(column=1, row=7, sticky=W)

# text fields
net = Entry(window)
net.grid(column=1, row=3, sticky=W)

vat = Entry(window)
vat.grid(column=1, row=4, sticky=W)

total = Entry(window)
total.grid(column=1, row=5, sticky=W)

# loop
window.mainloop()
