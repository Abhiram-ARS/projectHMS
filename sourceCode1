import mysql.connector
from tabulate import tabulate

print('='*100+'\n\n')
print('\n'*3)
print(' ------------------------------   HOSPITAL - MANAGEMENT - SYSTEM   ------------------------------')
print('\n'*3)
du=input(' '*40+'User-ID  :')
dp=input(' '*40+'Password :')
try:
    mydb=mysql.connector.connect(host="localhost",user=du,passwd=dp,database='Hospital_db')
    print('Database Acesssed')
    csr=mydb.cursor()
    print('='*100+'\n\n')
except:
    print('Error : Invalied User-ID or Password')
    exit()


while True:
    print('+----------------------------------+',
          '|            MAIN MENU             |',
          '+--------+-------------------------+',
          '| Choice |   Option                |',
          '+--------+-------------------------+',      
          '|   1    | Patient Sign-in         |',
          '|   2    | Patient Sign-up         |',
          '|   3    | Administrator log-in    |',
          '|   q    | Quit                    |',
          '+--------+-------------------------+',sep='\n',end='\n'*2)

    ch1=input('Enter Choice :-')
    print('='*100+'\n\n')
#----------------------------------------------------------------------------------------------------------------------------------------------------------
    if ch1=='q':       #quit
        print('Exiting from Hospital Management System...')
        enter=input('')
        exit()
        break
#----------------------------------------------------------------------------------------------------------------------------------------------------------    
    elif ch1=='1':            # Patient Sign-in
        cl=[]
        print(' --  Patient Sign-up --\n')
        csr.execute("select p_id from patients")
        for i in csr:
            cl.append(i[0])
        p=input("Enter P-id : P-")
        pid='P-'+p
        if pid in cl:
            csr.execute('select * from patients where p_id="'+pid+'"')
            for i in csr:
                print('\nPatient :',i)
            print('='*100+'\n\n')
            while True:
                print('+----------------------------------+',              # Patient MENU
                      '|        PATIENT MENU              |',
                      '+--------+-------------------------+',
                      '| Choice |   Option                |',
                      '+--------+-------------------------+',
                      '|   1    | Doctor Appointment      |',
                      '|   2    | Test Appointment        |',
                      '|   q    | Sign-out                |',
                      '+--------+-------------------------+',sep='\n',end='\n'*2)
                ch2=input('Enter Choice :-')
                print('='*100+'\n\n')
#----------------------------------------------------------------------------------------------------------------------------------------------------------
                if ch2=='q':
                    print('Patient Sign-out')
                    enter=input('')
                    break
#----------------------------------------------------------------------------------------------------------------------------------------------------------
                elif ch2=='1':
                    print(' --  Doctor Appointment --\n')
                    csr.execute('Select distinct(department) from doctors')
                    d_dep={}
                    x=1
                    for i in csr:
                        d_dep[str(x)]=i[0]
                        x+=1
                    print(tabulate(d_dep.items(),headers=['Department No','Depatrments'],tablefmt="fancy_grid"))
                    ch3=input('\nEnter Department No -')
                    print('='*100+'\n\n')
                    if ch3 in d_dep.keys():
                        department=d_dep[ch3]
                        print(department)
                        csr.execute('Select d_id,name from doctors where department="{}"'.format(department))
                        d_doc={}
                        for i in csr:
                            d_doc[i[0]]=i[1]
                        print(tabulate(d_doc.items(),headers=['D-id','Doctor'],tablefmt="fancy_grid"))
                        ch4_n=input('\nEnter Doctors ID : D-')
                        print('='*100+'\n\n')
                        ch4='D-'+ch4_n
                        if ch4 in d_doc.keys():
                            print('Doctor -',ch4,':',d_doc[ch4])
                            date=input('Enter Date of Appointment <yyyy-mm-dd> :')
                            print('='*100+'\n\n')
                            try:
                                csr.execute('Select count(*) from appointments where d_id="{}" and Date_of_Appointment="{}"'.format(ch4,date))
                                for i in csr:
                                    count_1=i[0]
                                
                                csr.execute('Select Daily_Patients from doctors where d_id="{}"'.format(ch4))
                                for i in csr:
                                    count_2=i[0]
                                if count_1<count_2:
                                    csr.execute('Select max(si_no) from appointments')
                                    for i in csr:
                                        sino=i[0]+1
                                    csr.execute('insert into appointments values({},"{}","{}",curdate(),"{}")'.format(str(sino),pid,ch4,date))
                                    print(csr.rowcount,'-Appointment Booked Sucessfully')
                                    mydb.commit()
                                else:
                                    print('Daily patient limit reached')
                            except:
                                print('Error : Invalied Date')
                        else:
                            print('Error : Invalied Doctor')
                        
                    else:
                        print('Error : Invalied Department')
                    print('='*100+'\n\n')
#----------------------------------------------------------------------------------------------------------------------------------------------------------
                elif ch2=='2':
                    print(' --  Test Appointment --\n')
                    csr.execute('Select * from test')
                    d_tst={}
                    for i in csr:
                        d_tst[i[0]]=i[1]
                    print(tabulate(d_tst.items(),headers=['Department No','Depatrments'],tablefmt="fancy_grid"))
                    ch3_n=input('\nEnter Test No : T-')
                    ch3='T-'+ch3_n
                    print('='*100+'\n\n')
                    csr.execute('Select max(si_no) from test_appointments')
                    for i in csr:
                        sino=i[0]+1
                    if ch3 in d_tst.keys():    
                        csr.execute('insert into test_appointments values({},"{}","{}",now())'.format(str(sino),pid,ch3))
                        print(csr.rowcount,'-Appointment Booked Sucessfully')
                        mydb.commit()
                    else:
                        print('Error : Invalied Choice')
                    print('='*100+'\n\n')
                    
                else:
                    print('Error : Invalied Choice')
                print('='*100+'\n\n')
        else:
            print('Error : Invalied P-id')
        print('='*100+'\n\n')
#----------------------------------------------------------------------------------------------------------------------------------------------------------        
    elif ch1=='2':            # Patient Sign-up
        print(' --  Patient Sign-up --\n')
        csr.execute("select max(p_id) from patients")
        for i in csr:
            id=int((i[0])[2:])+1
            pid='P-'+str(id)
        name=input('Enter Name :')
        age =input('Enter Age  :')
        ph_no=input('Enter Phone No :')
        print('='*100+'\n\n')
        q="insert into patients values('{}','{}',{},'{}')".format(pid,name,age,ph_no)
        try:
            csr.execute(q)
            mydb.commit()
            csr.execute('select * from patients where p_id="'+pid+'"')
            for i in csr:
                print(i)
            print(csr.rowcount,'-Patient added Sucessfully')
        except:
            print('Patient Sign-up Failed')
        print('='*100+'\n\n')
#----------------------------------------------------------------------------------------------------------------------------------------------------------        
    elif ch1=='3':              # Administrator log-in
        print(' ---  Administrator Log-in  ---\n')
        an=input(' Administrator Username :')
        ap=input(' Administrator Password :')
        print('='*100+'\n\n')
        while an=='name' and ap=='pwd':
            print('+-------------------------------------+',
                  '|        ADMINISTRATOR MENU           |',
                  '+--------+----------------------------+',
                  '| Choice |   Option                   |',
                  '+--------+----------------------------+',
                  '|   1    |   Find Patient             |',
                  '|   2    |   Patient Activity         |',
                  '|   3    |   Edit Patient             |',
                  '|   4    |   Manage Doctors           |',
                  '|   5    |   View from database       |',
                  '|   q    |   Quit                     |',
                  '+--------+----------------------------+',sep='\n',end='\n'*2)
            ch2=input('Enter Choice :-')
            print('='*100+'\n\n')
            if ch2=='q':
                print('Administrator Loging out..')
                enter=input('')
                print('='*100+'\n\n')
                break
#----------------------------------------------------------------------------------------------------------------------------------------------------------        
            elif ch2=='1': #FIND PATIENT
                print(' --  Find Patient  --\n')
                name=input("Enter Name :-")
                csr.execute('Select*from patients where name like "%{}%"'.format(name))
                rec=csr.fetchall()
                print("Records Found :-")
                if len(rec)==0:
                    print('\nNULL')
                else:
                    print(tabulate(rec,headers=['P_id','Name','Age','Phone_No'],tablefmt="fancy_grid"))
                print('='*100+'\n\n')
#----------------------------------------------------------------------------------------------------------------------------------------------------------
            elif ch2=='2': #PATIENT ACTIVITY
                print(' --  Patient Activity  --\n')
                p=input("Enter P-id : P-")
                pid='P-'+p
                print('* Patient Details :-\n\n')
                csr.execute('select * from patients where p_id="'+pid+'"')
                rec=csr.fetchall()
                if len(rec)==0:
                    print('Error : Invalied P-ID')
                else:
                    x=0
                    for i in ['P-ID','Name','Age','Phone_No']:
                        print(i,' :',rec[0][x])
                        x+=1
                    print('='*100+'\n\n')
                    print('* Doctor Appointments :-\n')
                    csr.execute('select a.Si_NO ,a.P_id,b.name,a.Date_of_Booking,a.Date_of_Appointment from appointments a,Doctors B where p_id="'+pid+'"and a.d_id=b.d_id')
                    rec=csr.fetchall()
                    print(tabulate(rec,headers=['Si_NO','P_id','Doctor','Date_of_Booking','Date_of_Appointment'],tablefmt="fancy_grid"))
                    print('='*100+'\n\n')
                    print('* Test Appointments :-\n')
                    csr.execute("select a.Si_NO,a.p_id,b.Test_name,a.Date_and_Time from test_appointments a, test b  where p_id='"+pid+"' and a.t_id=b.t_id")
                    rec=csr.fetchall()
                    print(tabulate(rec,headers=['Si_NO','P-ID','Test','Date_and_Time'],tablefmt="fancy_grid"))
#----------------------------------------------------------------------------------------------------------------------------------------------------------
            elif ch2=='3':  #EDIT PATIENT
                print(' --  Edit Patient  --\n')
                p=input("Enter P-id : P-")
                pid='P-'+p
                print('='*100+'\n\n')
                csr.execute('select * from patients where p_id="'+pid+'"')
                r=csr.fetchall()
                if len(r)==0:
                    print('Error : Invalied P-ID')
                            
                else:
                    print('\nPatient :',r)
                    print('='*100+'\n')
                    dt={'1':'Name','2':'Age','3':'Phone_no'}
                    print('CHANGE:-')
                    for i in dt.keys():
                        print('  [',i,']',dt[i])
                    ch4=input('Enter Choice :-')
                    print('='*100+'\n\n')
                    if ch4 in dt.keys():
                        new=input('Enter New Value :')
                        if ch4=='1':
                            csr.execute('update patients set {}="{}" where p_id="{}"'.format(dt[ch4],new,pid))
                        else:
                            csr.execute('update patients set {}={} where p_id="{}"'.format(dt[ch4],new,pid))
                        print(csr.rowcount,'-Patient updated Sucessfully')
                        mydb.commit()
                            
                    else:
                        print('Error : Invalied Choice')
#-------------------------------------------------------------------------------------------------------------------------------------------------                
            elif ch2=='4':
                print('+----------------------------------+',
                      '|        Manage Doctors            |',
                      '+--------+-------------------------+',
                      '| Choice |   Option                |',
                      '+--------+-------------------------+',
                      '|   1    |   Add Doctor            |',
                      '|   2    |   Edit Doctor           |',
                      '+--------+-------------------------+',sep='\n',end='\n'*2)
                ch3=input('Enter Choice :-')
                if ch3=='1':           # ADD DOCTOR
                    print(' --  Add Doctor  --\n')
                    csr.execute("select max(d_id) from doctors")
                    for i in csr:
                        id=int((i[0])[2:])+1
                        did='D-'+str(id)
                    name=input('Enter Name :')
                    dpt =input('Enter Departments :')
                    dp=input('Enter Daily Patients :')
                    print('='*100+'\n\n')
                    q="insert into doctors values('{}','{}','{}',{})".format(did,name,dpt,dp)
                    try:
                        csr.execute(q)
                        mydb.commit()
                        csr.execute('select * from Doctors where d_id="'+did+'"')
                        for i in csr:
                            print(i)
                        print(csr.rowcount,'-Doctor added Sucessfully')
                    except:
                        print('Add Doctor Failed')
                    print('='*100+'\n\n')
#-----------------------------------------------------------------------------------------------------------------------------------------                    
                elif ch3=='2':  #EDIT DOCTOR
                    print(' --  Edit Doctor  --\n')
                    d=input("Enter D-id : D-")
                    did='D-'+d
                    print('='*100+'\n\n')
                    csr.execute('select * from Doctors where d_id="'+did+'"')
                    r=csr.fetchall()
                    if len(r)==0:
                        print('Error : Invalied D-ID')
                                
                    else:
                        print('\nDoctor :',r)
                        print('='*100+'\n')
                        dt={'1':'Name','2':'Department','3':'Daily_Patients'}
                        print('CHANGE:-')
                        for i in dt.keys():
                            print('  [',i,']',dt[i])
                        ch4=input('Enter Choice :-')
                        print('='*100+'\n\n')
                        if ch4 in dt.keys():
                            new=input('Enter New Value :')
                            if ch4=='3':
                                csr.execute('update Doctors set {}={} where d_id="{}"'.format(dt[ch4],new,did))
                            else:
                                csr.execute('update Doctors set {}="{}" where d_id="{}"'.format(dt[ch4],new,did))
                            print(csr.rowcount,'-Doctor updated Sucessfully')
                            mydb.commit()
                                      
                else:
                    print('Error : Invalied Choice')
#-----------------------------------------------------------------------------------------------------------------------------------------
            elif ch2=='5':  #View from database
                print(' --  View from database  --\n')
                dbpwd=input('\nEnter Database Administrator Password :')
                while dbpwd=='database':
                    q=input('Enter Query  :  mysql>')
                    q=q.lower()
                    if q=='exit':
                        break
                    elif 'select' in q:
                        try:
                            csr.execute(q)
                            print("Executed")
                            rec=csr.fetchall()
                            print(tabulate(rec,tablefmt="fancy_grid"))
                            
                        except:
                            print('Error : Invalied Query')
                    else:
                        print('Error : Invalied Query')

                else:
                    print('Error : Invalied Database Administrator Password')    
#-----------------------------------------------------------------------------------------------------------------------------------------            else:
                print('Error : Invalied Choice')
            print('='*100+'\n\n')
        else:
            print('Error : Invalied Username or Password')
        print('='*100+'\n\n')

    else:
        print('Error : Invalied Choice')
    print('='*100+'\n\n')
