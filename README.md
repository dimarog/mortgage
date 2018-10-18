#!/usr/bin/python
'''
-------------------------------------------------------------------------
File name    : mortgage.py
Title        :
Project      :
Developers   :  $USER
Created      :
Description  :
Notes        :
---------------------------------------------------------------------------
Copyright 2018 (c) Qualcomm
---------------------------------------------------------------------------*/
'''

import math

# -----------------------------------------------
# globals
# -----------------------------------------------
MADAD = [
    0.03,-0.02,-0.08,-0.02,0.15,0.13,0.18,0.23,0.33,0.33,
    0.29,-0.04,0.25,0.08,-0.17,0.56,0.08,0.02,0.08,0.56,
    0.03,-0.02,-0.08,-0.02,0.15,0.13,0.18,0.23,0.33,0.33
]

# -----------------------------------------------
# -----------------------------------------------
def PMT(ir, np, pv, fv=0, ftype=False):
    '''
     * ir   - interest rate per month
     * np   - number of periods (months)
     * pv   - present value
     * fv   - future value
     * ftype - when the payments are due:
     *        False: Payments are due at the end of the period.
     *        True : Payments are due at the beginning of the period
     '''

    if ir == 0:
        return -(pv + fv)/np

    pvif = math.pow(1 + ir, np)
    pmt = -(ir * pv * (pvif + fv) / (pvif - 1))

    if ftype:
        pmt = pmt/(1 + ir)

    #return round(pmt,2)
    return pmt

# -----------------------------------------------
# -----------------------------------------------
def IPMT(pv, pmt, rate, per):
    tmp = math.pow(1 + rate, per)
    return 0 - (pv * tmp * rate + pmt * (tmp - 1))

def PPMT(rate, per, nper, pv, fv=0, ftype=False):
    if (per < 1) or (per >= nper + 1):
        return None
    pmt = PMT(rate, nper, pv, fv, ftype)
    ipmt = IPMT(pv, pmt, rate, per - 1)
    return pmt - ipmt



# -----------------------------------------------
# -----------------------------------------------
class Mortgage:
    def __init__(self, yearly_interest, months, amount):
        self._interest = float(yearly_interest)/(100*12)
        self._months = int(months)
        self._amount = amount
        self._curr_amount = amount

    def curr_amount(self,new_amount):
        self._curr_amount = new_amount

    def monthly_payment(self, month_num, curr_amount,curr_interest):
        return PMT(curr_interest,self._months - month_num, curr_amount)

    def monthly_interest_pay(self,curr_interest):
        return self._curr_amount * curr_interest

#    def monthly_fund_pay(self,total_monthly_payment, ):
#        return total_monthly_payment -


# -----------------------------------------------
# -----------------------------------------------
def maslulim():
    global spizer_lo_zmuda, spizer_zmuda
    spizer_lo_zmuda = Mortgage(3,360,1000000)
    spizer_zmuda = Mortgage(3,360,1000000)


# -----------------------------------------------
# -----------------------------------------------
def ppmt_sum(rate, nper, pv):
    sum = 0
    for i in range(1,nper+1):
        sum -= PPMT(rate,i,nper,pv)
        print (sum)

    return sum

# -----------------------------------------------
# -----------------------------------------------
def spizer_lo_zmuda_print():
    for i in range(20):
        monthly_payout = -spizer_lo_zmuda.monthly_payment(i, spizer_lo_zmuda._curr_amount,spizer_lo_zmuda._interest)
        print "retrun : "
        print monthly_payout
        print "interest return : "
        print spizer_lo_zmuda.monthly_interest_pay(spizer_lo_zmuda._interest)
        monthly_fund_return = monthly_payout - spizer_lo_zmuda.monthly_interest_pay(spizer_lo_zmuda._interest)
        spizer_lo_zmuda._curr_amount -= monthly_fund_return


# -----------------------------------------------
# -----------------------------------------------
def spizer_zmuda_print():
    for i in range(3):
        monthly_payout = -spizer_zmuda.monthly_payment(i, spizer_zmuda._curr_amount*(1+MADAD[i]/100),spizer_zmuda._interest)
        print "retrun : "
        print monthly_payout
        print "interest return : "
        print spizer_zmuda.monthly_interest_pay(spizer_zmuda._interest)
        monthly_fund_return = monthly_payout - spizer_zmuda.monthly_interest_pay(spizer_zmuda._interest)
        spizer_zmuda._curr_amount = spizer_zmuda._curr_amount*(1+MADAD[i]/100) - monthly_fund_return



# -----------------------------------------------
# -----------------------------------------------
def main():

    #
    maslulim()
    #
    #spizer_lo_zmuda_print()
    spizer_zmuda_print()



# -----------------------------------------------
# -----------------------------------------------
if __name__ == '__main__':
    main()



