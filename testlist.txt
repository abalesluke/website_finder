import requests, sys, time, os
from bs4 import BeautifulSoup
from colorama import Fore, Back, Style

subdomain = input('Enter sub/domain(default: .com): ')
if subdomain == '':
    subdomain = '.com'

os.system('clear')
print(Fore.MAGENTA+'opening wordlist...')
f = open('testlist.txt', 'r')
f = f.read().splitlines()
print(Fore.CYAN+'starting..\n')

dead = []
at = 0
dt = 0
for names in f:
    try:
        site = f'http://{names}{subdomain}'
        r = requests.get(site)

        soup = BeautifulSoup(r.content, features="html.parser")
        res = soup.find(content='See related links to what you are looking for.')
        if(res is None):
            res2 = soup.find(content="Let's Make Something Awesome")
            if(res2 is None):
                res3 = soup.find(content="text/html;")
                if res3 is None:
                    print(f'{Style.BRIGHT}{Fore.GREEN}[Active]: {Fore.YELLOW}{site}')
                    at += 1
                else:
                    dead.append(site)
                    dt+=1
            else:
                dead.append(site)
                dt+=1
        else:
            dead.append(site)
            dt+=1

    except:
        dead.append(site)
        dt+=1

if dead is None:
    print('\n\nnice! no dead sites.')

else:
    print(Back.MAGENTA+Fore.RED+"\n============DEAD=SITEs===================\n"+Style.RESET_ALL)
    for site in dead:
        print(f'{Style.BRIGHT}{Fore.RED}[Dead]: {Fore.MAGENTA}{site}{Style.RESET_ALL}')
    print(Back.MAGENTA+Fore.RED+"\n============DEAD=SITEs===================\n"+Style.RESET_ALL)
    
print(f'{Fore.GREEN}Active Total: {at}')
print(f'{Fore.RED}Dead Total: {dt}')
print(f'{Fore.CYAN}Overall Total: {at+dt}{Style.RESET_ALL}')
