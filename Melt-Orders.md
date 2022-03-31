#Acquisition
Melt Orders are received from L3 in the table **SDI_XFER.SRD_RECEIVE.MELT_ORDERS** processed by **LadleTracking Service** through the execution of the **LAD.Read_SRDMeltOrders** and **LAD.Write_MESMeltOrder** which in turns executes **MES.LAD.UpsertMeltOrder** that merges them into the table **LAD.MeltOrders**, creating rows in **LAD.Grades** and **CAT.Grades** if they don't exist.

LAD schema database tables are meant to be temporary, fast and short for ladle tracking, i.e. data is not completely validated because the priority is keep the flow going.

#Transfer to MES schema
Ladle Tracking Service executes the procedure **LAD.AssignMeltOrderToHeat** which in turns updates LAD.Heats with the proper LAD.MeltOrder but also upserts **MES.Heats, MES.Grades and MES.MeltOrders**

#Sending to L1
**Ladle Tracking Service** updates the following PLC registers 

![image.png](/.attachments/image-2d856bb5-4043-4e5c-97fe-2b8d469c10d0.png)

The **Global PLC Table** A list of Ladles in the LMF COMMON PLC

![image.png](/.attachments/image-03fbeed7-43cd-46d9-ae90-e83bcc9a660b.png)

A list of melt orders in the EAF COMMON PLC

![image.png](/.attachments/image-c3804b8c-b03f-4d15-b9bb-6e73e960e0a1.png)

A list of grades in the EAF COMMON PLC

![image.png](/.attachments/image-2917e085-49f0-40e7-99f3-21453420f0e0.png)

#Assignment of Heat to a Melt Order
## EAF HMI
When the EAF is loading the first charge he is asked for the Melt Order which he has to select from a list (pulled from EAF COMMON) or type it.

The EAF PLC sends a **HeatCommand** indicating that the heat has changed some of the attributes.  The MeltOrder has a grade so the Heat gets its Grade from there.

The HeatCommand is read by **Ladle Tracking Service** which in turn updates the **LAD.Heats** table and sends the new information to the **Global Ladles PLC table**

## Ladle Dashboard Screen
The user can also change the Heat/MeltOrder association at any point using this screen
![image.png](/.attachments/image-eb7c028c-f176-40ed-a6c7-0c91c0d4e637.png)
