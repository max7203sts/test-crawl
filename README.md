import requests
from bs4 import BeautifulSoup

def payback():
    url = 'https://rich01.com/jkos-pay-credit-cards/'
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')

    tables = soup.find_all('table')

    bank = {}
    payback = {}

    for table in tables:
        if '銀行' in table.get_text():
            bank_rows = table.find_all('tr')
            for row in bank_rows:
                bank_cells = row.find_all('td')
                if len(bank_cells) >= 2:
                    bank_name = bank_cells[0].get_text().strip()
                    bank_rate = bank_cells[1].get_text().strip()
                    bank[bank_name] = bank_rate

        if '%' in table.get_text():
            payback_rows = table.find_all('tr')
            for row in payback_rows:
                payback_cells = row.find_all('td')
                if len(payback_cells) >= 2:
                    payback_name = payback_cells[0].get_text().strip()
                    payback_rate = payback_cells[1].get_text().strip()
                    payback[payback_name] = payback_rate

    result = "Bank:\n"
    for bank_name, bank_rate in bank.items():
        result += f"{bank_name}: {bank_rate}\n"

    result += "\nPayback:\n"
    for payback_name, payback_rate in payback.items():
        result += f"{payback_name}: {payback_rate}\n"

    return result
