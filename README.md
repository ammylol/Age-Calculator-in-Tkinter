# Age Calculator in Tkinter GUI that will find your age accurately in Years, Months, Days, Hours, Minutes and Seconds

# importing all modules
from tkinter import *
import datetime
from calendar import leapdays
from calendar import isleap

# Creating a class and setting default value of class as Tk using 'super' builtin
class Age_Calculator(Tk):
    def __init__(self):
        super(Age_Calculator, self).__init__()

# title and geometry
        self.title('Age Calculator')
        self.geometry("190x420")
        self['bg'] = 'black'

# Creating heading label
        self.label = Label(text='Age Calculator', bg="black", fg="orange").grid(row=0, column=0, columnspan=3)

# Creating label for days, months and years
        self.label_date = Label(text='Date', bg="black", fg="red").grid(row=1, column=0)
        self.label_month = Label(text='Month', bg="black", fg="green").grid(row=1, column=1)
        self.label_year = Label(text='Year', bg="black", fg="blue").grid(row=1, column=2)
        self.label_hour = Label(text='Hour', bg="black", fg="pink").grid(row=3, column=0)
        self.label_minute = Label(text='Minute', bg="black", fg="purple").grid(row=3, column=1)

# Creating IntVar for storing integer
        self.var_date = IntVar()
        self.var_month = IntVar()
        self.var_year = IntVar()
        self.var_hour = IntVar()
        self.var_minute = IntVar()

# Entry for year input and setting textvariable = IntVar() means storing input integer in IntVar's variable
        self.entry_year = Entry(textvariable=self.var_year, width=10).grid(row=2, column=2)

# Getting current date to set default value for input
        self.current_date_for_entry = datetime.datetime.now()

# Setting Input place with default value
        self.var_date.set(self.current_date_for_entry.day)
        self.var_month.set(self.current_date_for_entry.month)
        self.var_year.set(self.current_date_for_entry.year)

# Creating button -- when button is clicked def(defination) Age will calculate age
        self.button = Button(text='Calculate', command=self.Age, bg="yellow").grid(row=5, column=0, columnspan=3)

# Entry for days input
        self.options_for_days = OptionMenu(self, self.var_date, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12,
                                     13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27,
        28, 29, 30, 31).grid(row=2, column=0)

# Entry for months input
        self.options_for_months = OptionMenu(self, self.var_month, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12)
        self.options_for_months.grid(row=2,column=1)

# Entry for minute input and setting textvariable = IntVar() means storing input integer in IntVar's variable
        self.entry_minute = Entry(textvariable=self.var_minute, width=10).grid(row=4, column=1)

# Entry for hours input
        self.options_for_hours = OptionMenu(self, self.var_hour, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15,
                                            16, 17, 18, 19, 20, 21, 22, 23)
        self.options_for_hours.grid(row=4, column=0)

    def Age(self):
# collected Integer from Entry(Input) and stored in IntVar()
        self.date = self.var_date.get()
        self.month = self.var_month.get()
        self.year = self.var_year.get()
        self.hour = self.var_hour.get()
        self.minute = self.var_minute.get()

# Getting current date to calculate the age
        self.current_date = datetime.datetime.today()

# Calculating age in years, months, days, hours and minutes
        self.calculated_date = (31 + self.current_date.day) - self.date
        self.calculated_month = ((12 + self.current_date.month) - self.month) - 1
        self.calculated_year = (self.current_date.year - self.year) - 1
        self.calculated_hour = (self.current_date.hour - self.hour)
        self.calculated_minute = (self.current_date.minute - self.minute)

# if current hour is smaller than input hour then calculated hour = current hour - (input hour - 24)
        if self.current_date.hour < self.hour:
            self.calculated_hour = (self.current_date.hour - (self.hour - 24))

# if input minute is bigger than current minute then calculated hour = calculated hour - 1 and
# calculated minute = input minute - current minute
        if self.minute > self.current_date.minute:
            self.calculated_hour -= 1
            self.calculated_minute = self.minute + self.current_date.minute

# if current hour is smaller than input hour then calculated date = calculated date - 1
        if self.current_date.hour < self.hour:
            self.calculated_date -= 1

# if current minute is smaller than input minute then calculated minnute = (60 - input minute) + current minute
        if self.current_date.minute < self.minute:
            self.calculated_minute = (60 - self.minute) + self.current_date.minute

# if input hour is equal to current hour and input minute is bigger than current minute then calculated hour = 23 and
# calculated date = calculated date - 1
        if self.hour == self.current_date.hour and self.minute > self.current_date.minute:
            self.calculated_hour = 23
            self.calculated_date -= 1

# Calculating all months the person lived in his whole life from years, months and days
        self.all_months = self.calculated_month + (self.calculated_year * 12)

# if current date is bigger or equal to input date then add 1 to all months
        if self.current_date.day >= self.date:
            self.all_months += 1

# Finding how many leap years are there between birth year and current year
        self.all_days_addon = leapdays(self.year, self.current_date.year)

# Calculating all days the person lived from years, months and days
        self.all_days = (self.calculated_year * 365) + (self.calculated_month * (365/12)) + self.calculated_date + \
                       self.all_days_addon

# if calculated days in years is bigger than 30 then calculated days in years = current date - birth date
        if self.calculated_date > 30:
            self.calculated_date = self.current_date.day - self.date

# if calculated months in years is bigger than 11 then calculated years = current year - birth year
# and calculated months in years = current month - birth month'''
        if self.calculated_month > 11:
            self.calculated_year = self.current_date.year - self.year
            self.calculated_month = self.current_date.month - self.month

# current month is bigger than birth month and current day is smaller that birth date then
# calculated months in years - 1
        if self.current_date.month > self.month and self.current_date.day < self.date:
            self.calculated_month -= 1

# if current month is smaller than birth month and current day is bigger than birth date then
# calculated month in years + 1
        if self.current_date.month < self.month and self.current_date.day > self.date:
            self.calculated_month += 1

# if current month in years = birth month and current day is bigger or equal to birth date then
# calculated month in year = 0 and calculated year + 1
        if self.current_date.month == self.month and self.current_date.day >= self.date:
            self.calculated_month = 0
            self.calculated_year += 1

# Checking that the birth year is leap year or not
        self.leapyearid = isleap(self.year)

# if birth year is a leap year and birth month is bigger than current month then
# all days the person lived + 1
        if self.leapyearid == True and self.month > self.current_date.month:
            self.all_days += 1

# if birth year is not a leap year and birth month is bigger than current month then
# all days the person lived + 2
        if self.leapyearid == False and self.month > self.current_date.month:
            self.all_days += 2

# if birth month is bigger than 7 then all days the person lived - 1
        if self.month > 7:
            self.all_days -= 1

# Calculating all hours the person lived in his whole life from all days the person lived and adding calculated hours
        self.all_hours = (int(self.all_days) * 24) + self.calculated_hour

# Calculating all minutes the person lived in his whole life from all hours the person lived and adding calculated minutes
        self.all_minutes = (self.all_hours * 60) + self.calculated_minute

# Calculating all seconds the person lived in his whole life from all minutes the person lived
        self.all_seconds = (self.all_minutes * 60) + self.current_date.second

# if birth date is smaller or equal to 31 and birth month is smaller or equal to 12 then
# Show the users their age
        if self.date <= 31 and self.month <= 12:
# Creating label to show the age
            self.Myage = Label(text=('Age: \n'
                                     + str(self.calculated_year) + '  Years, ' + str(self.calculated_month) +
                                     '  Months, ' + str(self.calculated_date) + '  Days\n' + str(self.calculated_hour) +
                                     '  Hours, ' + str(self.calculated_minute) + '  Minutes\n'
                                     + 'Or\n'
                                     + str(self.all_months) + '  Months, ' + str(self.calculated_date) + '  Days\n' +
                                     str(self.calculated_hour) + '  Hours, ' + str(self.calculated_minute) + '  Minutes\n'
                                     + 'Or\n'
                                     + '{:,}  Days, '.format(int(self.all_days)) +
                                     str(self.calculated_hour) + '  Hours, \n' + str(self.calculated_minute) + '  Minutes\n'
                                     + 'Or\n'
                                     + '{:,}  Hours, '.format(self.all_hours) +
                                     str(self.calculated_minute) + '  Minutes\n'
                                     + 'Or\n'
                                     + '{:,}  Minutes\n'.format(self.all_minutes)
                                     + 'Or\n'
                                     + '{:,}  Seconds\n'.format(self.all_seconds)
                                     + 'Old\n'),
                               bg="black", fg="white")
            self.Myage.grid(row=6, column=0, columnspan=3)

# if birth date = 31 and birth month has 30 days then show warning that there are only 30 days in this month
        if self.date == 31 and (self.month == 4 or self.month == 6 or self.month == 9 or self.month == 11):
            self.warning_label_1 = Label(text='There are only\n30 days in\n' + str(self.month) + 'th Month of year',
                                         bg="red")
            self.warning_label_1.grid(row=5, column=0, columnspan=3)

# if birth date is bigger than 28 and birth month is february and birth year is not leap year then
# show warning that there are only 28 days in february without leap year
        if self.date > 28 and self.month == 2 and self.leapyearid == False:
            self.warning_label_2 = Label(text='There are only\n28 days in\nFeburary Month', bg="red")
            self.warning_label_2.grid(row=5, column=0, columnspan=3)

# if birth date is bigger than 29 and birth month is february and birth year is leap year then
# show warning that there are only 29 days in february with leap year
        if self.date > 29 and self.month == 2 and self.leapyearid == True:
            self.warning_label_3 = Label(text='There are only\n29 days in\nFeburary Month of\nLeapyear', bg="red")
            self.warning_label_3.grid(row=5, column=0, columnspan=3)

# if birth date = current day and birth month = current month then wish the person 'happy birthday'
        if self.date == self.current_date.day and self.month == self.current_date.month:
            self.birthday = Label(text=('Happy Birthday\nto you on\nyour ' + str(self.calculated_year) + ' Birthday'),
                                  bg="yellow")
            self.birthday.grid(row=5, column=0, columnspan=3)

# Running the calculator
Age_Calculator().mainloop()
