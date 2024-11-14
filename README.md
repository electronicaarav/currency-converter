import pandas as pd
import requests

response = requests.get('https://api.exchangerate-api.com/v4/latest/USD')
exchange_rates = response.json()


rates = exchange_rates['rates']
df = pd.DataFrame(list(rates.items()), columns=['Currency', 'Rate'])
print(df)

def convert_currency(amount, from_currency, to_currency):
    from_rate = df.loc[df['Currency'] == from_currency, 'Rate'].values[0]
    to_rate = df.loc[df['Currency'] == to_currency, 'Rate'].values[0]
    converted_amount = amount * (to_rate / from_rate)
    return converted_amount


from_currency = input('From currency:')
amount = int(input('Enter the ammount:'))
to_currency = input('To Currency:')
converted_amount = convert_currency(amount, from_currency, to_currency)
print(f'{amount} {from_currency} is equal to {converted_amount:.2f} {to_currency}')
