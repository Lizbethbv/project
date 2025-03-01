# CS 3502-02 Project1 Multi-Threaded Programming and IPC
 using System;
 using System.Runtime.InteropServices.ComTypes;
 using System.Security.AccessControl;
 using System.Threading;
using System.Transactions;
using System.Xml.Serialization;



//mutex class
 class Mutex
{
    private bool isLocked = false;


    public void Lock()
    {
        while (isLocked)
        {
            Thread.Sleep(100); //thread waits for lock to be unlocked in certain tiem
        }

        isLocked = true;
    }

    public void Unlock()
    {
        isLocked = false;
    }
}

//synchronzied access in account class
    class Account

    {
        private double balance;
        private Mutex mutex = new Mutex();

        public Account(double balance1)
        {
            balance = balance1;
            ;


        }

        public void deposit(double amount)
        {
            mutex.Lock();
            balance += amount;

            Console.WriteLine("New Balance: " + balance);
            mutex.Unlock();
        }


        // }

        public void withdraw(double amount)
        {
            mutex.Lock();
            balance -= amount;
            Console.WriteLine("New Balance: " + balance+" ");
            mutex.Unlock();



        }

        public double Balance()
        {
            mutex.Lock();
            double balance2 = balance;
            mutex.Unlock();
            return balance2;

        }




    }


class Bank{


        static void Main(string[] args)
        {
            //multiple threads ona  shared bank account

            
            Account a = new Account(500);

            Thread t1 = new Thread(() =>
            {
                for (int i = 0; i < 5; i++)
                {
                    a.deposit(25);
                    Console.WriteLine($"Thread 1 deposited: {a.Balance()}");
                }
            });
            
           

            Thread t2 = new Thread(() =>
            {
                for (int i = 0; i < 5; i++)
                {
                    a.deposit(20);
                    Console.WriteLine($"Thread 2 deposited: {a.Balance()}");
                }
            });
            
            Thread t3 = new Thread(() =>
            {
                for (int i = 0; i < 5; i++)
                {
                    a.deposit(10);
                    Console.WriteLine($"Thread 3 withdrew: {a.Balance()}");
                }
            });

            
            
            
            
            t1.Start();
            t2.Start();
            t3.Start();
            
            
            t1.Join();
            t2.Join();
            t3.Join();
        }

    }
