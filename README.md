# HRM-FileStructures
Using C++ to manage and store human records in file structures.
#include<iostream.h>
#include<stdio.h>
#include<fstream.h>
#include<stdlib.h>
#include<string.h>
# include<conio.h>
class Person
{
char emp_id[30];
char emp_name[30];
char emp_address[30];
char position[30];
char department[30];
char buffer[100];
public:
void input();
void output();
void search();
void modify();
void pack();
void unpack();
void Write();
};
void Person :: input()
{
cout<<"Enter EMP_ID"<<endl;
cin>>emp_id;
cout<<"Enter Name"<<endl;
cin>>emp_name;
cout<<"Enter Address"<<endl;
cin>>emp_address;
cout<<"Enter position"<<endl;
cin>>position;
cout<<"Enter department"<<endl;
cin>>department;
}
void Person :: output()
{
istream& flush();
cout<<"EMP_ID:";
puts(emp_id);
cout<<"emp_Name :";
puts(emp_name);
 cout<<"emp_Address :";

puts(emp_address);
 cout<<"position :";
puts(position);
 cout<<"department :";
puts(department);
}
void Person::pack()
{
strcpy(buffer,emp_id); strcat(buffer,"|");
strcat(buffer,emp_name); strcat(buffer,"|");
strcat(buffer,emp_address); strcat(buffer,"|");
strcat(buffer,position); strcat(buffer,"|");
strcat(buffer,department); strcat(buffer,"|");
strcat(buffer,"#");
}
void Person::unpack()
{
char *ptr = buffer;
while(*ptr!='#')
{
if(*ptr == '|')
*ptr = '\0';
ptr++;
}
ptr = buffer;
strcpy(emp_id,ptr);
ptr = ptr +strlen(ptr)+1;
strcpy(emp_name,ptr);
ptr = ptr +strlen(ptr)+1;
strcpy(emp_address,ptr);
ptr = ptr +strlen(ptr)+1;
strcpy(position,ptr);
ptr = ptr+strlen(ptr)+1;
strcpy(department,ptr);
}
void Person:: Write()
{
fstream os("p.txt",ios::out|ios::app);
os.write(buffer,strlen(buffer));
os<<endl;
os.close();
}
void Person :: search()
{

int found = 0;
char key[30];
fstream is("p.txt",ios::in);
 cout<<"Enter The emp_id Of The Record To Be Searched "<<endl;
cin>>key;
while(!is.eof() && !found)
{
is.getline(buffer,'#');
unpack();
if(strcmp(emp_id,key) == 0)
{
cout<<"Record Found!!! "<<endl;
output();
found = 1;
}
}
if(!found)
cout<<"Record Not Found!!!"<<endl;
is.close();
}
void Person :: modify()
{
char key[30];
char del='$';
cout<<"Enter The emp_id Of The Record To Be Modified"<<endl;
cin>>key;
int found = 0;
fstream is;
is.open("p.txt",ios::in|ios::out);
while(!is.eof() && !found)
{
 is.getline(buffer,'#');
int len=strlen(buffer);
unpack();
if(strcmp(emp_id,key) == 0)
{
int pos=is.tellg();
pos=pos-len;
is.seekg(pos,ios::beg);
is<<del;
cout<<"ENTER 1:EMP_ID\n2:EMP_NAME\n3:EMP_ADDRESS \n4:POSITION \n 5:DEPARTMENT\n";
cout<<"Enter What to modify ? ";
int ch;
cin>>ch;
switch(ch)

{
case 1:
cout<<"EMP_ID :";
cin>>emp_id;
break;
case 2 :
cout<<"\nEMP_NAME :";
cin>>emp_name;
break;
case 3:
cout<<"\n POSITION :";
cin>>position;
break;
case 4:
cout<<"\nEMP_ADDRESS :";
cin>>emp_address;
break;
case 5:
cout<<"\n DEPARTMENT :";
cin>>department;
break;
default :
cout<<"wrong choice !!!";
break;
}
found = 1;
pack();
Write();
 }
}
if(!found)
cout<<"The Record with the given emp_id does not exist "<<endl;
is.close();
}
void main()
{
int choice = 1;
clrscr();
Person ob;
//istream& flush();
//ostream& flush();
//char filename[] = "p.txt";
while(choice < 4)
{
cout<<"1> Insert A Record "<<endl;
 cout<<"2> Search For A Record "<<endl;
 cout<<"3> Modify A Record "<<endl;
 cout<<"4> Exit "<<endl;
cin>> choice;
switch(choice)
{
case 1: ob.input();
ob.pack();
ob.Write();
break;
case 2: ob.search();
break;
case 3: ob.modify();
break;
}
} getch();
}
