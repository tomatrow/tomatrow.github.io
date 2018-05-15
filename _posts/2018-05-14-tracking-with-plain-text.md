So, it has come to my attention that adults keep track of their (1) time and (2) expenses. I wish to join in. Fortunately, I haven't really diving into this before. So I have no bad habits. 

What initially attracted me to this was [this manifesto](http://plaintextaccounting.org) endorsing plan text logging. 

Now, the trap I usually fall into is choosing the most complicated tool for the job, because, you know, *features*. Lets try to avoid that this time. There is a tool called [Ledger](https://www.ledger-cli.org) that fulfills both time and expense tracking. I can use a script called [t](https://github.com/nuex/t) to easily make time transactions. 

See, Ledger is *old*, like, really old. It's *so* old, that several new forks of it have matured into fully realized tools. Now, since none of these forks have replaced it, I'm guessing it's exactly what the common man needs. 


- - - 

Now, how do we go about using it? 

First, I need to choose a start date. To keep things simple, I'm picking today. Thus, a certain amount of money is just going to *show up* from the void in my initial account. 

Next, installation with syntax highlighting. That would be `brew install ledger`, choosing [`Ledger syntax highlighting`](https://github.com/moeffju/sublime-ledger-syntax) in package-control, and writing (or using) a script similar to [t](https://github.com/nuex/t). 

Some good introductions are [the official docs](https://www.ledger-cli.org/3.0/doc/ledger3.html) or this [blog post series](https://www.petekeen.net/keeping-finances-with-ledger). 

- - - 

Now that things are read and tools are installed, let's add our entries for time and money. 

We are going to need two files, what do I call them? I suppose their extensions shall be `.ledger` and `.timeclock`. They can both be named `journal`. So just run `touch .journal.ledger .journal.timeclock` in `~/`. 

Also, now that I've named them, it seems I can [easily set the default](https://www.ledger-cli.org/2.6/doc/ledger.html#Environment-variables) ledger. I'll set it to `~/.journal.ledger` since `t` will be my interface for `journal.timeclock`. So, we'll run `set -gx LEDGER_FILE ~/.journal.ledger` in `fish`. 

Now, running `ledger` opens our default. 

---

Before we get there, I'll need to decide on a structure. The [official recommendation](https://www.ledger-cli.org/3.0/doc/ledger3.html#Keeping-a-Journal) is as follows: 

* (1) Expenses: where money goes
* (2) Assets: where money sits
* (3) Income: where money comes from
* (4) Liabilities: money you owe
* (5) Equity: the real value of your property

I shall follow this for ease. 

According to [the docs](https://www.ledger-cli.org/3.0/doc/ledger3.html#Starting-up), I can add an opening balance in the following manner. 

```ledger
2018/04/29 Opening Balance
    Assets:Checking           $929.82
    Equity:Opening Balances
```

So, the first entries shall be...

```ledger
2018/04/29 14:36:00 Yellow Deli Drink Bar
    Expenses:food:coffeehouse           $2.17 ; Maté Tea 
    Assets:Checking
2018/04/29 17:30:00 Frazier Farm Markets ; For the Dairy-Free Lemon Ice Cream 
    Expenses:food:cooking               $12.42 ; Collagen 
    Assets:Checking
2018/04/30 18:57:00 NCTD
    Expenses:Transportation:Public      $5.00 ; Day Pass 
    Assets:Checking
2018/04/30 19:30:00 Banana Dang
    Expenses:food:coffeehouse           $3.68 ; Americano 
    Assets:Checking 
2018/04/30 11:38:00 Legoland Food Cart
    Expenses:food:default           $1.62 ; (accidental) Veggie burger, Guacamole, BBQ Sauce
    Assets:Checking 
2018/05/01 14:01:00 NCTD
    Expenses:Transportation:Public      $5.00 ; Day Pass 
    Assets:Checking
```

Also, when dealing with petty cash, [just ignore it](https://www.ledger-cli.org/3.0/doc/ledger3.html#Dealing-with-Petty-Cash).

---

Now, let's actually place the data in our journal and run some queries. 

```
> ledger balance
             $929.82  Assets:Checking
            $-929.82  Equity:Opening Balances
--------------------
                   0
```

```
> ledger accounts
Assets:Checking
Equity:Opening Balances
```

Pretty cool? 

---

Concerning Time: 

From the section on [Formatting](https://www.ledger-cli.org/3.0/doc/ledger3.html#Journal-Format), we see that I'll need to check out [`timeclock.el`](https://www.emacswiki.org/emacs/timeclock-x.el).

> I, i, O, o, b, h
> These four relate to timeclock support, which permits Ledger to read timelog files. See timeclock’s documentation for more info on the syntax of its timelog files.

See line 883 of [timeclock.el](https://github.com/emacs-mirror/emacs/blob/master/lisp/calendar/timeclock.el) for complete info. 
```
A timelog contains data in the form of a single entry per line.
Each entry has the form:
  CODE YYYY/MM/DD HH:MM:SS [COMMENT]
CODE is one of: b, h, i, o or O.  COMMENT is optional when the code is
i, o or O.  The meanings of the codes are:
  b  Set the current time balance, or \"time debt\".  Useful when
     archiving old log data, when a debt must be carried forward.
     The COMMENT here is the number of seconds of debt.
  h  Set the required working time for the given day.  This must
     be the first entry for that day.  The COMMENT in this case is
     the number of hours in this workday.  Floating point amounts
     are allowed.
  i  Clock in.  The COMMENT in this case should be the name of the
     project worked on.
  o  Clock out.  COMMENT is unnecessary, but can be used to provide
     a description of how the period went, for example.
  O  Final clock out.  Whatever project was being worked on, it is
     now finished.  Useful for creating summary reports.
```

I don't see the `I` option represented in `timeclock.el`; assuming it's just "Initial Clock In" or something. 

I'll need to make a task tree similar to the account tree. 

```
side-projects
    spanreed 
    coffeefilter
ministry
    Jr High 
    YAmondays
chores 
    dishes 
    laundry
    cleaning
media
    reading 
        reproof
            Fearless
        Fantasy/Sci-Fi
            Jonathan Strange & Mr Norrell
    Podcasts 
        Biblical
            Hope Generation
        Technical
        Fantasy/Sci-Fi
            WOT Spoilers 
            The Legendarium
    Shows 
        Fantasy/Sci-Fi
        Marvel Universe
```

Now, an example. 

CODE YYYY/MM/DD HH:MM:SS [COMMENT]
```
i 2018/05/14 09:45:00 // Legoland Education Associate
o 2018/05/14 17:00:00 
i 2018/05/14 19:00:00 // Landmark
o 2018/05/14 21:00:00
```

Which produces

```
> ledger -f .journal.timeclock balance
               2.00h  // Landmark
               7.25h  // Legoland Education Associate
--------------------
               9.25h
```

and 

```
> ledger -f .journal.timeclock accounts
// Landmark
// Legoland Education Associate
```

Of course, time tracking will be restricted to *actual* projects.