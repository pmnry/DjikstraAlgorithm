# DjikstraAlgorithm
Course Project for the "C++" class at ENSAE Paristech
Authors: Etienne Kintzler and Pierre Monroy

import os
import pandas as pd

columns = ['adults', 'zipCode', 'legal', 'noNights','tax','name', 'startRent', 'endRent', 'website', 'totalRent',
           'websiteFees', 'websiteFeesPayment', 'creditCardFees', 'creditCardFeesPayment', 'cityTax', 'cityTaxPayment',
           'misc', 'miscPayment', 'rentNetFees', 'todrop1', 'todrop2', 'todrop3', 'todrop4', 'todrop5', 'todrop6',
           'todrop7', 'todrop8', 'amountPerceived', 'amountPerceivedMethod', 'todrop9', 'todrop10', 'todrop11', 
           'todrop12', 'conciergeFeesTaxExcl', 'conciergeFeesTaxIncl', 'ownerRevenue', 'ownerDue', 'totalCol', 
           'spendingDetail', 'spendingAmount']

path = r'/home/pmonroy/Appt'

files = os.listdir(path)

files_xls = [f for f in files if f[-3:] == 'xls']

dfs = pd.DataFrame()

for f in files_xls:
    data = pd.read_excel(path + '/' + f, 'COMPTES DETAIL', skiprows=[0,1], names=columns)
    data = data.dropna(subset=['adults'])
    dfs = dfs.append(data.iloc[:-1, :])
    
dfs['startRent'] = pd.to_datetime(dfs['startRent'])
dfs.sort_values('startRent')

cols = [col for col in dfs.columns if col[:6] != 'todrop']
dfs = dfs[cols]
