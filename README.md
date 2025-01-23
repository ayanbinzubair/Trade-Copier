#include <iostream>
using namespace std;

const int maxpositions = 10;
const int maxaccounts = 3;

struct trade 
{
    string tradeID;
    string assetsymbol;
    double vol;
    string type;
};

struct account 
{
    string accountname;
    double risk;
    trade trades[maxpositions];
    int tradecount = 0;
};

void addTrade(trade masteraccountpositions[], int& masteraccountpositionscount) 
{
    if (masteraccountpositionscount >= maxpositions) 
    {
        cout << "Cannot add more trades. Master account is full.\n";
        return;
    }

    trade& newtrade = masteraccountpositions[masteraccountpositionscount];

    cout << "Enter Trade ID: ";
    cin >> newtrade.tradeID; 

    cout << "Enter Asset Symbol: ";
    cin >> newtrade.assetsymbol;

    cout << "Enter Volume (positive number): ";
    cin >> newtrade.vol;
    while (newtrade.vol <= 0) 
    {
        cout << "Invalid input. Enter a positive volume: ";
        cin >> newtrade.vol;
    }

    cout << "Enter Type (Buy/Sell): ";
    cin >> newtrade.type;
    while (newtrade.type != "Buy" && newtrade.type != "Sell" && newtrade.type != "buy" && newtrade.type != "sell" ) 
    {
        cout << "Invalid input. Enter 'Buy' or 'Sell': ";
        cin >> newtrade.type;
    }

    masteraccountpositionscount++;
    cout << "Trade added successfully.\n";
}

void displayTrades(trade masteraccountpositions[], int masteraccountpositionscount) 
{
    if (masteraccountpositionscount == 0) 
    {
        cout << "No trades to display.\n";
        return;
    }

    for (int i = 0; i < masteraccountpositionscount; i++) 
    {
        cout << "Trade ID: " << masteraccountpositions[i].tradeID << endl;
        cout << "Symbol: " << masteraccountpositions[i].assetsymbol << endl;
        cout << "Volume: " << masteraccountpositions[i].vol << endl;
        cout << "Type: " << masteraccountpositions[i].type << endl;
    }
}

void addAccount(account slaveaccounts[], int& slaveaccountscount) 
{
    if (slaveaccountscount >= maxaccounts) 
    {
        cout << "Cannot add more accounts. Limit reached.\n";
        return;
    }

    account& newAccount = slaveaccounts[slaveaccountscount];

    cout << "Enter Account Name: ";
    cin >> newAccount.accountname;

    cout << "Enter Risk Multiplier (e.g., 1.0 for no change, 2.0 for double risk, positive numbers only): ";
    cin >> newAccount.risk;
    while (newAccount.risk <= 0) 
    {
        cout << "Invalid input. Enter a positive risk: ";
        cin >> newAccount.risk;
    }

    slaveaccountscount++;
    cout << "Account added successfully.\n";
}

void displayAccounts(account slaveaccounts[], int slaveaccountscount) 
{
    if (slaveaccountscount == 0) 
    {
        cout << "No accounts to display.\n";
        return;
    }

    for (int i = 0; i < slaveaccountscount; i++) 
    {
        cout << "Account Name: " << slaveaccounts[i].accountname << endl;
        cout << "Risk Multiplier: " << slaveaccounts[i].risk << endl;
        displayTrades(slaveaccounts[i].trades, slaveaccounts[i].tradecount);
    }
}

void copyTrades(trade masteraccountpositions[], int masteraccountpositionscount, account slaveaccounts[], int slaveaccountscount) 
{
    if (masteraccountpositionscount == 0) 
    {
        cout << "No trades to copy in the master account.\n";
        return;
    }

    if (slaveaccountscount == 0) 
    {
        cout << "No accounts to copy trades to.\n";
        return;
    }

    for (int i = 0; i < slaveaccountscount; i++) 
    {
        account& currentAccount = slaveaccounts[i];
        currentAccount.tradecount = 0;

        for (int j = 0; j < masteraccountpositionscount; j++) 
        {
            if (currentAccount.tradecount >= maxpositions) 
            {
                cout << "Trade limit reached for account: " << currentAccount.accountname << endl;
                break;
            }
            else
            {
                currentAccount.trades[currentAccount.tradecount] = masteraccountpositions[j];
                currentAccount.trades[currentAccount.tradecount].vol *= currentAccount.risk;
                currentAccount.tradecount++;
            }
        }

        cout << "Trades copied to account: " << currentAccount.accountname << endl;
    }
}

int main() 
{
    trade masteraccountpositions[maxpositions];
    int masteraccountpositionscount = 0;

    account slaveaccounts[maxaccounts];
    int slaveaccountscount = 0;

    int choice;

    do 
    {
        cout << "Trade Copier Menu:\n";
        cout << "1. Add Trade to Master Account\n";
        cout << "2. Display Master Trades\n";
        cout << "3. Add New Account\n";
        cout << "4. Display Accounts and Their Trades\n";
        cout << "5. Copy Trades to Accounts\n";
        cout << "0. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) 
        {
            case 1:
                addTrade(masteraccountpositions, masteraccountpositionscount);
                break;
            case 2:
                displayTrades(masteraccountpositions, masteraccountpositionscount);
                break;
            case 3:
                addAccount(slaveaccounts, slaveaccountscount);
                break;
            case 4:
                displayAccounts(slaveaccounts, slaveaccountscount);
                break;
            case 5:
                copyTrades(masteraccountpositions, masteraccountpositionscount, slaveaccounts, slaveaccountscount);
                break;
            case 0:
                cout << "Exiting program. Thank you!\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 0);

    return 0;
}
