def main(quit):
    print("=====================================")
    print("Welcome to Hann's Bank Service System")
    print("=====================================")
    while True:
        print("Would you like to log in as:")
        print("\n\t1. SuperUser")
        print("\t2. Admin")
        print("\t3. Customer")
        choice = input("Please insert your choice here: ")
        if choice == '1':
            if SystemLogin() == "Login Successfully!":
                Super_User_Menu()
        if choice == '2':
            if SystemLogin() == "Login Successfully!":
                Admin_Menu()
        if choice == '3':
            if Customer_Login() == "Great! You've successfully logged into your account!":
                Customer_Menu()
        else:
            print("Invalid choice! Please try again!")

def SystemLogin():
    print("Do remember to add A in front of your ID if your're an admin or S if you're a SuperUser")
    with open("system_account_details.txt") as s:
        system_details = s.readlines()
        details_list = [line.strip() for line in system_details]
        details_list = list(map(str, details_list))
        username = input("Please enter your username here: ")
        if username in details_list:
            index = details_list.index(username)
            realpassword = details_list[index + 1]
            password = input("Please enter your password here: ")
            if password == realpassword:
                confirmation = "Login Successfully!"
                return confirmation
        else:
            print("Login failed! Please try again!")
                        

def Super_User_Menu():
    while True:
            print("SUPER USER MENU")
            print("===============")
            print("\n\t1. Add new admin staff account")
            print("\t2. Log out from the system")
            ans = input("Please enter your choice: ")
            if ans == "1":
                  Add_Staff()
            elif ans == "2":
                  print("Have a nice day ahead! Hope to see you soon!")
                  break
            else:
                  print("Invalid choice! Please try again!")
                
def Add_Staff():
      with open('system_account_details.txt') as system_handler:
          system = system_handler.readlines()
          system_account = [line.strip() for line in system]
          if len(system_account) == 2:
              new_system_account = 'A0000000001'
          else:
              last_system_account = f'{system_account[-2][1:]}'
              counter = str(int(last_system_account) + 1)
              new_system_account = counter.zfill(10)
          print("Your staff ID is:", new_system_account)
          print("Please insert a default password for your staff account!")
          account_password = input()
          with open('system_account_details.txt', 'a') as system_handler1:
              system_handler1.write('A' + f'{new_system_account}' + '\n')
              system_handler1.write(f'{account_password}' + '\n')

def Admin_Menu():
    while True:
        print("Welcome! What would you like to do?")
        print("\n\t 1. Register a new customer account")
        print("\t 2. Update customer's details")
        print("\t 3. Change account password")
        print("\t 4. Exit")
        choices = input("Enter your choices here:")
        if choices == '1':
            Customer_Account()
            Bank_Account_Number()
            Default_Account()
        if choices == '2':
            Update()
        if  choices == '3':
            Change_Password()
        if choices == '4':
            print("Have a nice day ahead! Hope to see you soon!")
            break
        else:
            print("Invalid choice! Please try again!")

def Customer_Account():
    print("Hi there! Please fill in your personal details to move on")
    print("Please enter your name!")
    name = input()
    print("Please enter your contact number!")
    contact = input()
    print("Please enter your address!")
    address = input()
    password = name.replace(' ','') + str(contact[6:10])
    print("Your password is :", password)
    with open("customer_account_password.txt" , "a") as file_handler:
#This will be able to match the password with the username 
        file_handler.write(str(password) + '\n')
        file_handler.write('*' + '\n')
        file_handler.write('*' + '\n')
    namelist = open("customer_personal_details.txt" , "a")
    namelist.write(name + '\n')
    namelist.close()
    contactlist = open("customer_personal_details.txt" , "a")
    contactlist.write(contact + '\n')
    contactlist.close()
    addresslist = open("customer_personal_details.txt" , "a")
    addresslist.write(address + '\n')
    addresslist.close()



def Bank_Account_Number():
    with open('customer_account_number.txt') as customer_handler1:
        numbers = customer_handler1.readlines()
        account_numbers = [line.strip() for line in numbers]
        if len(account_numbers) == 1:
            last_account_number = int(account_numbers[0])
        else:
            last_account_number = int(account_numbers[-3])
        counter = f'{last_account_number + 1}'
        new_account_number = counter.zfill(10)
        print("Your account number is:", new_account_number)    
        with open('customer_account_number.txt', 'a') as f:
            f.write(f'{new_account_number}' + '\n')
        print("Great! You've successfully created an account!")




def Default_Account():
    print("Hi there! Which type of account would you like to create today?")
    print("For Savings Account, Choose S \nFor Current Account, Choose C")
    choices = input()
    if choices == 'S':
        print("Do bank in RM100 as the minimum balance to create this account") 
        with open('customer_account_number.txt', 'a') as customer_handler1:
              customer_handler1.write('100.00' + '\n')
              customer_handler1.write('0.00' + '\n')
        return
    if choices == 'C':
        print("Do bank in RM500 as the minimum balance to create this account")
        with open('customer_account_number.txt', 'a') as customer_handler1:
              customer_handler1.write('0.00' + '\n')
              customer_handler1.write('500.00' + '\n')
        return
    
def Update():
    Name = input("Please enter the customer's name:")
    with open("customer_personal_details.txt") as details_handler:
        customer_details = details_handler.readlines()
        customer_details_list = [line.strip() for line in customer_details]
        index = customer_details_list.index(Name)
        print("What details would you like to update?")
        print("\n\t1.Contact Number \n\t2.Address")
        choices = input("Enter your choice here: ")
        if choices == '1':
            Old_Contact = customer_details_list[index + 1]
            New_Contact = input("Enter your new contact number: ")
            customer_details_list[index + 1] = New_Contact
        if choices == '2':
            Old_Address = customer_details_list[index + 2]
            New_Address = input("Enter your new address: ")
            customer_details_list[index + 2] = New_Address
        print("Details updated!")
        with open("customer_personal_details.txt", "w") as details_handler1:
            for details in customer_details_list:
                emp = ''
                emp = emp + f'{details}'
                details_handler1.write(f'{emp}' + '\n')
                
def Customer_Login():
    with open("customer_account_number.txt") as  a, open("customer_account_password.txt") as p:
        number = a.readlines()
        password = p.readlines()
        lns = [line.strip() for line in number]
        lns_2 = [line.strip() for line in password]
#This will find the index of the account number in the txt file
#The map() function executes a specified function for each item in an iterable
        lns = list(map(float, lns))
        accountnumber = int(input("Enter your bank account number: "))
        if accountnumber in lns:
#This step is to find the specific index of the password in the txt file
            index = lns.index(accountnumber)
            realpassword = lns_2[index]
            print("Great!")
            accountpassword = input("Enter your bank account password: ")
            if accountpassword == realpassword:
                with open('customer_personal_details.txt') as d, open('customer_account_number.txt') as a:
                    name = d.readlines()
                    numbers = a.readlines()
                    customer_name = [line.strip() for line in name]
                    account_numbers = [line.strip() for line in numbers]
                    account_numbers = list(map(float, lns))
                    name_index = account_numbers.index(accountnumber)
                    realname = customer_name[name_index]
                    confirmation = "Great! You've successfully logged into your account!"
                    print(f"Hi {realname}, what would you like to do now?")
                    return confirmation
            else:
                print("Wrong password, please try again!")
        else:
            print("Bank account number not found, please try again!")
            
def Customer_Menu():
    while True:
        print("\n\t 1. Transaction")
        print("\t 2. Display statement of account")
        print("\t 3. Change Account Password")
        print("\t 4. Exit")
        choices = input("Enter your choice here: ")
        if choices == '1':
            Transaction()
        if choices == '2':
            Account_Statement()
        if choices == '3':
            Change_Password()
        if choices == '4':
            print("Have a nice day ahead! Hope to see you soon!")
            break
        else:
            print("Invalid choice! Please try again!")
    
def Transaction():
    import datetime
    date = datetime.date.today()
    print("\n\t1.Withdrawal \n\t2.Deposit \n\t3.Transfer \n\t4.Exit")
    following_choices = input("Enter your choice: ")
    with open("customer_account_number.txt") as account_handler, open("customer_account_password.txt") as password_handler:
        account = account_handler.readlines()
        password = password_handler.readlines()
        account_list = [line.strip() for line in account]
        password_list = [line.strip() for line in password]
        password = list(map(str, password_list))
        print("Please verify your password again to continue")
        account_password = input()
        while True:
            if account_password in password_list:
                index = password_list.index(account_password)
                print("Please choose the type of account!")
                print("For Savings account, press S, for Current Account, press C")
                choices = input()
                if choices == 'S':
                    Old_Savings = float(account_list[index + 1])
                    if following_choices == '1' :
                        print("Insert the amount of money you woudld like to withdraw from!")
                        withdraw_amount = float(input())
                        Balance = Old_Savings - withdraw_amount
                        account_list[index + 1] = Balance
                        if Balance < 100:
                              print("Insufficient amount! Unable to proceed!")
                        else:
                              print("Withdrawal completed! Your current balance amount is:", Balance)
                              with open('customer_account_number.txt', 'w') as withdraw_handler:
                                  for details in account_list:
                                      emp = ''
                                      emp = emp + f'{details}'
                                      withdraw_handler.write(f'{emp}' + '\n')
                              with open("customer_account_number.txt") as account_handler, open("statement_of_account.txt", "a") as statement_handler:
                                  account_number = account_list[index]
                                  statement = f'{account_number}' + ' withdrawn ' +  f'{withdraw_amount}' + ' from ' + ' savings ' + ' on ' + f'{date}'
                                  statement_handler.write(f'{statement}' + '\n')
                    if following_choices == '2':
                        print("Insert the amount of money you would like to deposit!")
                        deposit_amount = float(input())
                        Balance = Old_Savings + deposit_amount
                        account_list[index + 1] = Balance
                        print("Deposit completed! Your current balance amount is:", Balance)
                        with open("customer_account_number.txt", "w") as deposit_handler:
                            for details in account_list:
                                emp = ''
                                emp = emp + f'{details}'
                                deposit_handler.write(f'{emp}' + '\n')
                        with open("customer_account_number.txt") as account_handler, open("statement_of_account.txt", "a") as statement_handler:
                          account_number = account_list[index]
                          statement = f'{account_number}'+ ' deposited ' +  f'{deposit_amount}' + ' to ' + ' savings ' + ' on ' + f'{date}'
                          statement_handler.write(f'{statement}' + '\n')
                    if following_choices == '3':
                            print("Please insert the account number that you would want to transfer to!")
                            target_account_number = input()
                            account_index = account_list.index(target_account_number)
                            print("Please insert the amount you would like to transfer!")
                            Transfer_Amount = float(input())
                            Savings_Balance = Old_Savings - Transfer_Amount
                            if Savings_Balance < 100:
                                print("Insufficient amount! Unable to proceed")
                            else:
                                account_list[index + 1] = Savings_Balance
                                account_choice = input("Which type of account would you like to transfer to? For Savings Account, press S, For Current Account, press C: ")
                                if account_choice == 'S':
                                    Target_Savings = float(account_list[account_index + 1])
                                    if Target_Savings == 0:
                                        print("Bank account doesn't exist!")
                                        Transaction()
                                    else:
                                        Target_Balance = Target_Savings + Transfer_Amount
                                        account_list[account_index + 1] = Target_Balance
                                elif account_choice == 'C' :
                                    Target_Current = float(account_list[account_index + 2])
                                    if Target_Current == 0:
                                        print("Bank account doesn't exist!")
                                        Transaction()
                                    else:
                                        Target_Balance = Target_Current + transfer_amount
                                        account_list[index +2] = Target_Balance
                                print("Transfer completed! Your current balance is:", Savings_Balance)
                                with open('customer_account_number.txt', 'w') as transfer_handler:
                                      for details in account_list:
                                          emp = ''
                                          emp = emp + f'{details}'
                                          transfer_handler.write(f'{emp}' + '\n')
                                with open("customer_account_number.txt") as account_handler, open("statement_of_account.txt", "a") as statement_handler:
                                  account_number = account_list[index]
                                  statement1 = f'{account_number}' + ' transferred ' +  f'{Transfer_Amount}' + ' to ' + f'{target_account_number}' + ' on ' + f'{date}'
                                  statement2 = f'{target_account_number}' + ' received ' + f'{Transfer_Amount}' + ' from ' + f'{account_number}' + ' on ' + f'{date}'
                                  statement_handler.write(f'{statement1}' + '\n')
                                  statement_handler.write(f'{statement2}' + '\n')
                    if following_choices == '4':
                        Customer_Menu()
                    Transaction()
                if choices == 'C':
                    Old_Current = float(account_list[index + 2])  
                    if following_choices == '1' :
                        print("Insert the amount of money you woudld like to withdraw from!")
                        withdraw_amount = float(input())
                        Balance = Old_Current - withdraw_amount
                        account_list[index + 2] = Balance
                        if Balance < 100:
                              print("Insufficient amount! Unable to proceed!")
                        else:
                              print("Withdrawal completed! Your current balance amount is:", Balance)
                              with open('customer_account_number.txt', 'w') as withdraw_handler:
                                  for details in account_list:
                                      emp = ''
                                      emp = emp + f'{details}'
                                      withdraw_handler.write(f'{emp}' + '\n')
                              with open("customer_account_number.txt") as account_handler, open("statement_of_account.txt", "a") as statement_handler:
                                  account_number = account_list[index]
                                  statement = f'{account_number}' + ' withdrawn ' +  f'{withdraw_amount}' + ' from ' + ' current ' + ' on ' + f'{date}'
                                  statement_handler.write(f'{statement}' + '\n')
                    if following_choices == '2':
                        print("Insert the amount of money you would like to deposit!")
                        deposit_amount =float(input())
                        Balance = Old_Current + deposit_amount
                        account_list[index + 2] = Balance
                        print("Deposit completed! Your current balance amount is:", Balance)
                        with open("customer_account_number.txt", "w") as deposit_handler:
                            for details in account_list:
                                emp = ''
                                emp = emp + f'{details}'
                                deposit_handler.write(f'{emp}' + '\n')
                        with open("customer_account_number.txt") as account_handler, open("statement_of_account.txt", "a") as statement_handler:
                          account_number = account_list[index]
                          statement = f'{account_number}'+ ' deposited ' +  f'{deposit_amount}' + ' to ' + ' current ' + ' on ' + f'{date}'
                          statement_handler.write(f'{statement}' + '\n')
                    if following_choices == '3':
                        print("Please insert the account number that you would want to transfer to!")
                        target_account_number = input()
                        account_index = account_list.index(target_account_number)
                        print("Please insert the amount you would like to transfer!")
                        Transfer_Amount = float(input())
                        Current_Balance = Old_Current - Transfer_Amount
                        if Current_Balance < 100:
                            print("Insufficient amount! Unable to proceed")
                        else:
                            account_list[index + 2] = Current_Balance
                            print("Which type of account would you like to transfer to? For Savings Account, press S, For Current Account, press C")
                            if account_choice == 'S':
                                Target_Savings = float(account_list[account_index + 1])
                                if Target_Savings == 0:
                                    print("Bank account doesn't exist!")
                                    Transaction()
                                else:
                                    Target_Balance = Target_Savings + Transfer_Amount
                                    account_list[account_index + 1] = Target_Balance
                            elif account_choice == 'C' :
                                Target_Current = float(account_list[account_index + 2])
                                if Target_Current == 0:
                                    print("Bank account doesn't exist!")
                                    Transaction()
                                else:
                                    Target_Balance = Target_Current + Transfer_Amount
                                    account_list[index +2] = Target_Balance
                            print("Transfer completed! Your current balance is:", Current_Balance)
                            with open('customer_account_number.txt', 'w') as transfer_handler:
                                  for details in account_list:
                                      emp = ''
                                      emp = emp + f'{details}'
                                      transfer_handler.write(f'{emp}' + '\n')
                            with open("customer_account_number.txt") as account_handler, open("statement_of_account.txt", "a") as statement_handler:
                              account_number = account_list[index]
                              statement1 = f'{account_number}' + ' transferred ' +  f'{Transfer_Amount}' + ' to ' + f'{target_account_number}' + ' on ' + f'{date}'
                              statement2 = f'{target_account_number}' + ' received ' + f'{Transfer_Amount}' + ' from ' + f'{account_number}' + ' on ' + f'{date}'
                              statement_handler.write(f'{statement1}' + '\n')
                              statement_handler.write(f'{statement2}' + '\n')
                    if following_choices == '4':
                        break
                else:
                    print("Wrong Password inserted please try again!")
            break
            

def daterange(date1, date2):
    from datetime import timedelta, date
    for n in range(int ((date2 - date1).days)+1):
        yield date1 + timedelta(n)
#timedelta is to find the difference between two dates
                                      
def Account_Statement():
    while True:
        from datetime import timedelta, date
        account_number = input("Enter bank account number: ")
        print("Please insert the range of date according to YYYY-MM-DD")
        start_date = input("Enter your start date: ")
        end_date = input("Enter your end date: ")
        start_Year = int(start_date[0:4])
        start_Month = int(start_date[5:7])
        start_Day = int(start_date[8:10])
        end_Year = int(end_date[0:4])
        end_Month = int(end_date[5:7])
        end_Day = int(end_date[8:10])
        start = date(start_Year, start_Month, start_Day)
        end = date(end_Year, end_Month, end_Day)
        print("============Statement of Account============")
        with open("statement_of_account.txt") as statement_handler:
            statement_of_account = statement_handler.readlines()
            statement_list = [line.strip() for line in statement_of_account]
            statement_list = list(map(str, statement_list))
            for d in daterange(start, end):
                for statement in statement_list:
                    statement_string = str(statement)
                    required_dates = d.strftime("%Y-%m-%d")
                    if account_number == statement_string[0:10]:
                        if required_dates  == statement_string[-10:]:
                           print(f'{statement}' + '\n')
            print("=======================================")
            break
   
def Change_Password():
    while True:
        print('Please confirm the old password')
        Old_Password = input()
        with open("customer_account_password.txt") as customer_password_handler, open("system_account_details.txt") as system_password_handler:
            customer_password = customer_password_handler.readlines()
            system_password = system_password_handler.readlines()
            customer_password_list = [line.strip() for line in customer_password]
            system_password_list = [line.strip() for line in system_password]
            customer_password = list(map(str, customer_password_list))
            system_password = list(map(str, system_password_list))
            print("Please insert a new password!")
            New_Password1 = input()
            print("Please reconfirm your password!")
            New_Password2 = input()
            if New_Password1 == New_Password2:
                if Old_Password in customer_password_handler:
                    customer_index = customer_password_list.index(Old_Password)
                    customer_password_list[customer_index] = New_Password1
                    with open('customer_account_password.txt', 'w') as password_handler1:
                        for passwords in customer_password_list:
                            emp = ''
                            emp = emp + f'{passwords}'
                            password_handler1.write(f'{emp}' + '\n')
                elif Old_Password in system_password_handler:
                    system_index = system_password_list.index(Old_Password)
                    system_password_list[system_index] = New_Password1
                    with open("system_account_details.txt") as password_handler2:
                        for passwords in system_password_list:
                            emp = ''
                            emp = emp + f'{password}'
                            password_handler2.write(f'{emp}' + '\n')
                print("Password successfully changed! \nDon't tell anyone your password!")
            else:
                print("The passwords do not match! Please try again!")
            break
                      
#This is to allow many inputs and it wouldn't break the third time
if __name__ == "__main__" :
    quit = False
    while quit == False:
        quit = main(quit)

