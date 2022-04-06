#Basic data
Location E:\Australtek\MES\LecoService 
Log files are in E:\Australtek\MES\logs

#Authorization
Make sure the service is logged with an account with permissions over the share
![image.png](/.attachments/image-4b6843ae-3895-4f3a-ab4f-73b2acf9ddd1.png)

#Configuration file 
appsettings.json

![image.png](/.attachments/image-60f6d02c-77a3-43e9-b84f-44540b4f79f9.png)

#View Database Logs
`select * from system.log where [Procedure]='LAB.UpdateLecoN'`

![image.png](/.attachments/image-e1bf1b65-54cb-412d-80ec-f6d816655d08.png)