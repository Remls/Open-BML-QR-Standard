# Open-BML-QR-Standard

[READ THE WIKI](https://github.com/WhoIsFishie/Open-BML-QR-Standard/wiki)

## Goals
- Set a QR code schema that all the shops in the country can follow. This will allow developers to create Android and iOS applications that will follow the schema as a standard, so that there will be no clashes when different people use different apps.

- Making QR codes that allow users to share personal banking details safely and efficiently

- Create an open source Android and iOS app that will scan the QR code easily

## Requirements
- The data of the user must fit into the QR code
- The data must be able to serialize and de-serialize without issue on any programming language of choice
- The data must be readable even when scanned normally (*this means the data **MUST NOT** be compressed to bypass requirement 1*)
- Must not use any **proprietary** software as this is an Open Standard (*this rules out Microsoft Tags*)
- Data must be readable offline (*the data can't point to a website which then loads the details*)

### Current Suggestions
- [Heavy Json](#heavy-json)
- [Lite Json](#lite-json)
- [INI](#ini)
- [RAW 1](#raw-1)
- [CSV](#csv)
- [NText](#ntext)

## Examples of the given suggestions can be seen below
<br>
<br>
<br>

#### Heavy Json

![Heavy Json](Img/Heavy-Json.png)

```jsonc
{
    "BML_Data": [
        { 
            "AccountName": "Company Name - MVR",
            "AccountNumber": "7770-0000-12345",
            "AccountType": 0 //MVR
        },
        { 
            "AccountName": "Company Name - USD",
            "AccountNumber": "7770-0000-54321",
            "AccountType": 1 //USD
        }
    ],
    "Contacts": [
        { 
            "Token": "Telegram",
            "Value": "@Username"
        },
        { 
            "Token": "Viber",
            "Value": "+960 712 3456"
        }
    ],
    "Extra": [
        { 
            "Token": "Memo",
            "Value": "Please Include Your Invoice Number In the Remarks"
        }
    ],
    "Version": 1
}
```
This option allows multiple types of data to be added in a very flexible way. It also supports multi account and multi contacts.

------------

<br>



#### Lite Json

![Lite-Json](Img/Lite-Json.png)

```jsonc
{
    "Ver": "2.0",
    //BML Account Number
    "AccNum": "7770-0000-54321",
    // Account Name as displayed on BML
    "Name": "Company Name - MVR",
    /// 1 = MVR
    /// 0 = USD
    "Type": 0,
    // contact number to send the payment slip to
    "Numb": "+960 712 3456"
}
```
This is a simpler and lighter version of Heavy Json, but it only supports single contact and account.

------------

<br>



#### INI

![INI](Img/Ini.png)

```ini
Ver=3
//BML Account Number
AccNum=7770-0000-54321
// Account Name as displayed on BML
Name=Company Name - MVR
/// 1 = MVR
/// 0 = USD
Type=0
//contact number to send the payment slip to
Numb=+960 712 3456
```
An alternative to json as there are less characters being used.

------------

<br>



#### RAW 1

![RAW1](Img/RAW1.png)

```
//BML Account Number
7770-0000-54321
Company Name
1
+960 712 3456
```
This is a raw text version where regex is used to detect contact number and account number. A single digit is assigned to the currency, and the account name will be the only string.

------------

<br>



#### CSV

![CSV](Img/CSV.png)

```
7770-0000-54321,7770-0000-12345
Company Name,Company Name
1,0
Viber,Telegram
+960 712 3456,@username
```
An updated version of RAW 1.

This method uses CSV and Regex as well as line numbers to phase the text. This method is very readable but not easily scalable. In place of CSV we could also use TSV.

------------

<br>



#### NText

![NText](Img/NText.png)

```
BANK:
    A:
        >Company Name
        >7777000001234
        >0
    B:
        >Company Name
        >7777000001234
        >1
viber:7123456
```
[Nested Text](https://nestedtext.org/en/stable/) is a simpler and much more readable alternative to JSON 
