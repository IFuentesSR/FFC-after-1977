# FFC-after-1977

from datetime import datetime, date, time

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import sys

#read csv file
Read=pd.read_csv(r'C:\Users\ifue3702\Downloads\419003 (2).csv', 
                 header=None, 
                 names=['Date','Discharge volume', 'Quality'], 
                 index_col=False, 
                 skiprows=range(0,4), 
                 na_values="")
#remove na's and 0's
Read_a=Read.dropna(subset=['Discharge volume'])
Read_b=Read_a[Read_a['Discharge volume']>0]
#sort descending
Read_c=Read_b.sort_values(['Discharge volume'], ascending=False)

#Split data (after 1978)
#remove time of date in new column
Read_c['New_Date']=[s.replace('00:00:00 ', '') for s in Read_c['Date']]
#create new column with datetype objects
Read_c['Dates']=pd.to_datetime(Read_c.New_Date)
#Define threshold date to select data
Threshold=date(1977,12,31)
#Select data after the threshold
After=Read_c[Read_c.Dates>Threshold]
#sorting data
After_sorted=After.sort_values(['Discharge volume'], ascending=False)
#Add new column with index of order
After_sorted['New_Order']=range(1,len(After_sorted.index)+1)
#Add new colum with calculated Exccedance probability
After_sorted['New_Exceedance_Probability']=After_sorted.New_Order/(len(After_sorted.index))

#plotting the curves
New=plt.plot(After_sorted['New_Exceedance_Probability'], After_sorted['Discharge volume'])
plt.gca().invert_xaxis()
plt.xlabel('Exceedance Probability')
plt.ylabel('Discharge Volume (ML)')
plt.savefig('After to 1977.png')
