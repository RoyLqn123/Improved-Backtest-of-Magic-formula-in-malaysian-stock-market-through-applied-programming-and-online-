# Improved-Backtest-of-Magic-formula-in-malaysian-stock-market-through-applied-programming-and-online-
Improved Backtest of Magic formula in malaysian stock market through applied programming and online quantitative platform
# Research environment functions imports 
from quantopian.research import run_pipeline 

# Pipeline imports 
from quantopian.pipeline.data import EquityPricing # OHLCV data from Global equity
from quantopian.pipleine.data import factset # Factset data set 

from quantopian.pipeline.domain import MY_EQUITIES # Malaysian equities 

from quantopian.pipeline import Pipeline 
from quantopian.pipeline.data.factset import RBICSFosus 

# Other libraries imports 
import matplotlib.pyplot as plt
import pandas as pd 
import numpy as np

import datetime
from date time import date 
from dateutil.relativedelta import relativedelta 


# Get fundamental data fro all listed company 
def Get_fundamental_data(time,):
    start_period = time 
    end_period = start_period 
    
    Pipe = Pipeline(
           Columns = {
                     'EBIT':factset.Fundamentals.ebit_oper_af.latest, 
                     'Enterprise value':factset.Fundamentals.entrpr_val_af.latest,
                     'Total assets':factset.Fundamentals.assets_af.latest,
                     'Total current assets': factset.Fundamentals.assets_curr_af.latest,
                     'Intangible assets': factset.Fundamentals..intang_af.latest,
                     'Working capital': factset.Fundamentals.wkcap_af.latest,
                     'Market cap':factset.Fundamentals.mkt_val.latest,
                     
                     'listing currency': factset.Equity metadat.listing_currency.latest,
                     'Security type': factset.EquityMetadata.security_type.latest,
                     
                     
                     
                     },
                     domain = MY_EQUITIES)
                     
    fdm_data = run_pipeline(
                 pipe,
                 start_period,
                 end_period)
                 
                 
                 
                 
                 
                 
# Data pre-processing 

    fdm_data = fdm_data.dropna()
    
    tickers = fdm_data.index.get_level_values(1)
    
    fdm_data['company_name'] = [ticker.asset_name for ticker in tickers] 
    
    fdm_data = fdm_data.sort_values(ascending = False, by 'mkt_cap')
    
    volume_1 = len(fdm_data) 
    
    return fdm_data 
    
    
# Set first filter( Company market capitalization) 

def Filter_fin_uti(result): 

    result = result[result['mkt_cap'] > 1000000000
    result = result[result['EBIT'] > 0
    
# Exclude Financial company 

    result = result[result['company_name']! = 'Alliance Bank Malaysia Bhd.']
    result = result[result['company_name']! = 'AEON Credit Service(M) Bhd.']
    result = result[result['company_name']! = 'Affin Bank Bhd.']
    result = result[result['company_name']! = 'Allianz Malaysia Bhd.']
    result = result[result['company_name']! = 'AMMB Holdings Bhd.']
    result = result[result['company_name']! = 'Apex Equity Holdings Bhd.']
    result = result[result['company_name']! = 'BIMB Holdings Bhd.']
    result = result[result['company_name']! = 'Bursa Malaysia Bhd.']
    result = result[result['company_name']! = 'CIMB Group Holdings Bhd']
    result = result[result['company_name']! = 'ECM lIBRA gROUP Bhd.']
    
    result = result[result['company_name']! = 'ELK-Desa Resources Bhd.']
    result = result[result['company_name']! = 'Fintec Global Bhd.']
    result = result[result['company_name']! = 'Hong Leong Bank Bhd.']
    result = result[result['company_name']! = 'Hong Leong Financial Group Bhd.']
    result = result[result['company_name']! = 'Insas Bhd.']
    result = result[result['company_name']! = 'Kenanga Investment Bank Bhd.']
    result = result[result['company_name']! = 'Kuchai Development Bhd.']
    result = result[result['company_name']! = 'LPI Capital Bhd.']
    result = result[result['company_name']! = 'MAA Group Bhd.']
    result = result[result['company_name']! = 'Manulife Holdings Bhd.']
    
    result = result[result['company_name']! = 'Malayan Banking Bhd.']
    result = result[result['company_name']! = 'Malaysia Building Society Bhd.']
    result = result[result['company_name']! = 'MNRB Holdings Bhd.']
    result = result[result['company_name']! = 'MPHB Capital Sdn.Bhd.']
    result = result[result['company_name']! = 'OSK Ventures International Bhd.']
    result = result[result['company_name']! = 'Pacific & Orient Bhd.']
    result = result[result['company_name']! = 'TA Enterprise Bhd.']
    
    result = result[result['company_name']! = 'Syarikat Takaful Malaysia Keluarga Bhd.']
    result = result[result['company_name']! = 'Tune Protect Group Bhd.']
    
    result = result[result['company_name']! = 'Brite-Tech Bhd.']
    result = result[result['company_name']! = 'Eden, Inc. Bhd.']
    result = result[result['company_name']! = 'Gas Malaysia Bhd.']
    result = result[result['company_name']! = 'Malakoff Corporation Bhd']
    result = result[result['company_name']! = 'Mega First Corp.Bhd.']
    result = result[result['company_name']! = 'PBA Holdings Bhd.']
    result = result[result['company_name']! = 'Petronas Gas Bhd.']
    result = result[result['company_name']! = 'Ranhill Utilities Bhd.']
    result = result[result['company_name']! = 'Salcon Bhd.']
    result = result[result['company_name']! = 'Taliworks Corp. Bhd']
    
    result = result[result['company_name']! = 'Tenaga Nasional Bhd']
    result = result[result['company_name']! = 'YTL Corp Bhd.']
    result = result[result['company_name']! = 'YTL Power International Bhd.']
    
    return result 
    
    
    
# Calculate Return of Capital and Earing Yield 

def rank_roc_ey(result):
    result['ROC'] = result.apply(lambda x : x ['EBIT']/ (x['Total assets'] - ['Total current assets'] 
                                            - x['Intangible assets'] + ['Working capital']), axis = 1)
    
    result['EY'] = result.apply(lambda x: x['EBIT'] / X['Enterprise value'], axis = 1 )
    
    rank_ROC = result['ROC'].rank(ascending = False, method = 'average') 
    
    rank_EY = result ['EY'].rank(ascending = False, method = 'average') 
    rank_used = rank_ROC + rank_EY 
    
    rank_used = pd.DataFrame(rank_used).rename(columns = {0:'rank'})
    new_result = pd.merge(result,rank_used,left_index = True, right_index = True) 
    
    new_result = new_result.sort_values(by = 'rank') 
    
    new_result.reset_index(drop = True, inplace = True) 
    my_portfolio = new_result.head(30)
    
    return my_portofolio 
    
    
# Get next year price for selected company 

def next_price(out_date):
    start_period_p = out_date 
    end_period_p = out_date 
    
    pipe = Pipeline(
          columns = { 'Dividend': factset.Fundamentals.dps_qf.latest, 
                      'Price': EquityPricing.colse.latest,
                      'listing currency': factset.EquityMetadata.listing_currency.latest,
                      'security type': factset.EquityMetadata.security_type.latest
                    },
                      domain = MY_EQUITIES) 
                      
          result_p = run_pipeline(
                     pipe,
                     start_period_p,
                     end_period_p)
                     
          result_P = result_p.dropna()
          
          tickers = result_P.index.get_level_values(1)
          
          result_P['company_name'] = [ticker.asset_name for ticker in tickers] 
          
          return result_p 
          
          
 # Merge two years price data 
 
 def meg_data(my_portfolio, result_p):
 
     ff_result = pd.merge(my_portofolio, result_p, how = 'left', on 'company_name')
     
     ff_result = ff_result.dropna()
     
     return ff_result 
     
 def cal_dividend(result):
 
    result['f_dividend'] = result.apply(lambda x : x['Dividend_y'] - x['Dividend_x'], axis =1) 
    
    result[result<0] = np.nan 
    result = result.fillna(0) 
    
    return result 
    
    
# Calculate the rate of return 

 def cal_revenue_d(ff_result):
 
     ff_result['Percentage_change'] = ff_result.apply(
        lambda x:(x['price_y'] - x['price_x'] + x['f_dividend'] / x['price_x'], axis = 1)
        
        
     revenue = sum(ff_result.Percentage_change)/30
     
     return revenue 
     
     
# Adjust the timing 

 def get_year(year,):
 
     start_period = str(year) +'-04-15'
     
     return start_period 
     
 def get_next_month(start_period): 
     
     text = start_period
     time_name = datetime.datetime.strptime(text, '%Y-%m-%d') 
     this_months = time_name + relativedelta(months = +1)
     end_period = this_months 
     next_month = end_period 
     
     return next_month 
     
     
     
# The full Magic formula 

  def magic_formula(year):
      start_period = get_year(year)
      result = Get_fundamental_data(start_period)
      filted_result = Filter_fin_uti(result)
      my_portofolio = rank_roc_ey(filted_result) 
      
      result_p0 = next_price(start_period) 
      month_1 = get_next_month(start_period)
      result_p1 = next_price(month_1)
      f_result1 = meg_data(f_result1,result_p0)
      ff_result1 = meg_data(f_result1,result_p1)
      ff_result1 = cal_dividend(ff_result1)
      return_1 = cal_revenue_d(ff_result1) 
    
      month_2 = month_1 + relativedelta(months = +1) 
      result_p2 = next_price(month_2)
      f_result2 = meg_data(my_portfolio,result_p2)
      ff_result2 = meg_data(f_result2,result_p2)
      ff_result2 = cal_dividend(ff_result2)
      return_2 = cal_revenue_d(ff_result2) 
      
      month_3 = month_1 + relativedelta(months = +1) 
      result_p3 = next_price(month_3)
      f_result3 = meg_data(my_portfolio,result_p3)
      ff_result3 = meg_data(f_result3,result_p3)
      ff_result3 = cal_dividend(ff_result3)
      return_3 = cal_revenue_d(ff_result3) 
    
      month_4 = month_1 + relativedelta(months = +1) 
      result_p4 = next_price(month_4)
      f_result4 = meg_data(my_portfolio,result_p4)
      ff_result4 = meg_data(f_result4,result_p4)
      ff_result4 = cal_dividend(ff_result4)
      return_4 = cal_revenue_d(ff_result4) 
    
      month_5 = month_1 + relativedelta(months = +1) 
      result_p5 = next_price(month_5)
      f_result5 = meg_data(my_portfolio,result_p5)
      ff_result5 = meg_data(f_result4,result_p5)
      ff_result5 = cal_dividend(ff_result5)
      return_5 = cal_revenue_d(ff_result5) 
      
      month_6 = month_1 + relativedelta(months = +1) 
      result_p6 = next_price(month_6)
      f_result6 = meg_data(my_portfolio,result_p6)
      ff_result6 = meg_data(f_result6,result_p6)
      ff_result6 = cal_dividend(ff_result6)
      return_6 = cal_revenue_d(ff_result6)
      
      month_7 = month_1 + relativedelta(months = +1) 
      result_p7 = next_price(month_7)
      f_result7 = meg_data(my_portfolio,result_p7)
      ff_result7 = meg_data(f_result7,result_p7)
      ff_result7 = cal_dividend(ff_result7)
      return_7 = cal_revenue_d(ff_result7) 
    
      month_8 = month_1 + relativedelta(months = +1) 
      result_p8 = next_price(month_8)
      f_result8 = meg_data(my_portfolio,result_p8)
      ff_result8 = meg_data(f_result8,result_p8)
      ff_result8 = cal_dividend(ff_result8)
      return_8 = cal_revenue_d(ff_result8) 
    
      month_9 = month_1 + relativedelta(months = +1) 
      result_p9 = next_price(month_9)
      f_result9 = meg_data(my_portfolio,result_p9)
      ff_result9 = meg_data(f_result9,result_p9)
      ff_result9 = cal_dividend(ff_result9)
      return_9 = cal_revenue_d(ff_result9)     
       
      month_10 = month_1 + relativedelta(months = +1) 
      result_p10 = next_price(month_10)
      f_result10 = meg_data(my_portfolio,result_p10)
      ff_result10 = meg_data(f_result10,result_p10)
      ff_result10 = cal_dividend(ff_result10)
      return_10 = cal_revenue_d(ff_result10)         
    
      month_11 = month_1 + relativedelta(months = +1) 
      result_p11 = next_price(month_11)
      f_result11 = meg_data(my_portfolio,result_p11)
      ff_result11 = meg_data(f_result11,result_p11)
      ff_result11 = cal_dividend(ff_result11)
      return_11 = cal_revenue_d(ff_result11)             
    
      month_12 = month_1 + relativedelta(months = +1) 
      result_p12 = next_price(month_12)
      f_result12 = meg_data(my_portfolio,result_p12)
      ff_result12 = meg_data(f_result12,result_p12)
      ff_result12 = cal_dividend(ff_result12)
      return_12 = cal_revenue_d(ff_result12)          
      
      return return_1, reutrn_2, return_3, return_4, return_5, return_6,
             return_7, return_8, return_9, return_10, return_11, return_12
      
    
