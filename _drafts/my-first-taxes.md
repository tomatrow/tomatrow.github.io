# My First Taxes 

Please note we are palying on the *my-first-x* for baby toys.


## Tasks 

* (00) Find the third-party sites they gave me. 
* (01) Determine the documents I need to fill out.
* (02) General research on the nature of taxes. 
* (03) Make general decisions. 
* (04) Specifically research each form.
* (05) Actually fill out forms. 

## Resources 

* [r/personalfinance](https://www.reddit.com/r/personalfinance/)
* [1040 form with tooltips](http://apps.urban.org/1040/2015/f1040-1.html)
* Eric Johnson 
* Alex Hebert 
* HR?

## Log 

### Step 00 

According to various emails; I've found the following: Dayforce, I-9, Transamerica, Perks-at-work (*ugh*), when-to-work. Only Dayforce and Transamerica are tax relevant. 

Seems Dayforce is concerned with all the business-logic not covered in the scheduler. This includes: forms (*sweet*), verifying the hours log, payment. These forms include personal info, policy/general agreements, and tax documents. 

Thus, Dayforce is our home today. 

### Step 01 

There are four forms listed on Dayforce. 

* DE4.California - 2018: "The California DE4 form is used to complete state tax withholding elections for the state of California"
* Direct Deposit: "The Direct Deposit form updates details for direct deposit of pay and other compensation"
* Federal W4 - 2017: "The Federal W4 form is used to complete a US Federal W-4 tax form"
    - It seems most of it is a *worksheet* with *only* one required field. 
* Province/State Tax Form: This is *literally* the *exact same* form as **DE4.California - 2018**, *no difference* whatsoever.

Direct deposit is obvious enough. 

Hopefully I can find something on r/personalfinance for each document. 

### Step 02 

I found [a wiki entry](https://www.reddit.com/r/personalfinance/wiki/young_adult) directed at me on r/personalfinance. This led to a more direct [page on taxes](https://www.reddit.com/r/personalfinance/wiki/taxes) and [ELI5 on 401k](https://www.reddit.com/r/personalfinance/wiki/401k). I also should probably take a look at r/almosthomeless. 

As a special note; all these forms look *fake*. As in, my internal vision of *tax forms* consists of a gray-scale, gothic, series of obscure semi-complex words that require hours-of-googling to make sense of. 


#### r/personalfinance's Taxes Wiki 

#### Introduction to The Wonders of Taxes 

They ask us to note that things will change significantly in the new 2018 tax year. They also want us to know this guide is not exhaustive.

I need to do taxes since my income will be above \$400. I have a few options on *how* I can do that:

* (1) I can pay someone (or some service)
* (2) Use paper forms
* (3) Use a web application
* (4) Use a more *serious* desktop application

Yet, I'll still need a separate solution for *state* taxes (*ugh*).

#### The General Strategy 

I want more money, so I'll try to minimize taxes; as one does.

There are two (somewhat vaguely separated) avenues for that: Deductions and Exemptions. 

##### Deductions

Deductions reduce the portion of your income that can be taxed. There are a few different instances of this minimization strategy. 

* (1) The **Standard Deduction**: It's a *standardized* chunk of ~$6,350 that's removes from your taxable income, but only for single (like me) filers.
* (2) The **Itemized Deductions**: These are for common singular expenses, including (*strangely*) State Taxes and Tithing. 
* (3) The **Above The Line Deductions**: These the *big and serious* items; e.g. student loans, self-employment costs. 

Note that you *must choose* to take the standard (1) or itemized (2,3) deductions. I'll calculate itemized, but I'll probably take standard. 


##### Exemptions 

Exemptions are itemized deductions where the *items* are people. 

See [table 3-1](https://www.irs.gov/publications/p17#idm140565607775056) for an overview. 

As an example in Eric's context: 

* Clare is the spouse. +1 
* Catherine is physically disabled. +1
* Carrissa is a student between 19-24. +1 
    - Slightly not sure if her school qualifies as a *school*.
* Tim I'm not sure about. $\frac{1}{2} \pm \frac{1}{2}$
    - He *did* take some MiraCosta classes $\thn$  +1.

See [irs p17 ch03](https://www.irs.gov/publications/p17#en_US_2017_publink1000170839) for more info. 

#### Concerning Income Brackets

Tax brackets are a ordered partition of your income equipped with their own tax rate. 

So, if your income is $r \in \R^\ge$ and your taxable income is $s \le r$ then the government defines a partition $P = \{P_i = (a_i,b_i)\}$ over $r$ and function $f: P \to \R^\ge$ such that your taxes are $\sum_{j} (b_j - a_j) \cdot f(P_j)$. That's why we minimize $s$. 


#### On Tax Withholding 

r/personalfinance suggests my withholding should be as accurate as possible, and be aware of [some-kinda-penalty](https://www.irs.gov/taxtopics/tc306) for *under-withholding* ($\iff$ you causing *your employer* to withhold to little). 

I'm not sure the penalty is actually enforced based on Eric's advice. 

Alex said let my employer withhold as must as possible for ease of mind. 

### Step 03 

Decisions, desiccations...no, condescension...not that, condescensions...no, ...Decisions...yes.

As one does (apparently), I'll aim for control and minimization. 

* (1) Minimize taxable income via Standard or Itemized methods 
* (2) Aim to under-withhold within non-penalize-able limits. 

(1) $\thn$ Less taxable income $\thn$ more money at the end of the year (*yay*).
(2) $\thn$ I'm in control of my money for as long as possible $\thn$ I can invest and get returns (*double yay*).


### Step 04 

Concerning each and every form I need to fill out. 

I have only two really. One is a duplicate of California's withholding form. The other is just a *non-offical* series of fields. 

* Form W-4 (2017): The federal withholding form, found [here](https://www.irs.gov/pub/irs-prior/fw4--2017.pdf). 
* DE 4 Rev. 46 (12-17): The state withholding form, found [here](http://www.taxes.ca.gov/de4.pdf). 

#### W4 

##### Some observations 
This years (similar) form is [here](https://www.irs.gov/pub/irs-pdf/fw4.pdf). 

So, looking at the form itself; the only part that matters is the middle portion. The top is *just* a worksheet. The bottom worksheet is used of you chose itemization over standard deduction. 


##### Specific fields

I really only need those basic info fields (*easy peasy*), (5) is completely determined by the worksheet, (7) is a definite no, (6) seems weird. 

#### DE 4

##### Some observations 

This form seems divided into four portions. The first is the actual fields we care about; the rest is three worksheets labeled A - C. 

##### Specific fields

For Worksheet A I'll really just claim myself. +1

I'm really not sure about worksheets B and C; especially C. 

### Step 05 

Left as an exercise to the reader. 