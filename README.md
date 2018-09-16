# 9.16.2
个人银行储蓄账户的实现
// 9.16.1个人银行账户管理系统.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include<iostream>
#include<cmath> //标准C++库文件
using namespace std;

class SavingAccounts { //储蓄账户类
private:
	int id; //账号
	double balance; //余额
	double rate; //存款的年利率
	int lastdate; //上次变更余额的日期
	double accumulation;//余额按日累加之和

//记录每一笔账，date为日期，amount为金额，desc为说明
	void record(int date, double amount) ;
//获得到指定日期为止的存款金额按日累计值
	double accumulate(int date) const {
		return accumulation + balance * (date - lastdate);
	}
public:
	//构造函数
	SavingAccounts(int date, int id, double rate);
	int getID() { return id; }
	double getBalance() { return balance; }
	double getRate() { return rate; }
	//存入现金
	void deposit(int date, double amount);
	//取出现金
	void withdraw(int date, double amount);
	//结算利息，每年1月1日调用一次该函数
	void settle(int date);
	//显示账户信息
	void show();
};
//SavingAccounts类相应成员函数的实现
SavingAccounts::SavingAccounts(int date, int id, double rate) :id(id),balance(0),lastdate(date),rate(rate),accumulation(0){
	cout << date << "\t#" << id << "is created" << endl;
}
void SavingAccounts::record(int date, double amount) {
	accumulation = accumulate(date);
	lastdate = date;
	amount = floor(amount * 100 + 0.5) / 100;//保留小数点后两位
	balance += amount;
	cout << date << "\t#" << id << "\t" << amount << "\t" << balance << endl;
}
void SavingAccounts::deposit(int date, double amount) {

	record(date,amount);
}
void  SavingAccounts::withdraw(int date, double amount){
	
	record(date, -amount);
}
void SavingAccounts::settle(int date) {
	double interest = accumulate(date) * rate / 365; //计算年息
	if (interest != 0)
		record(date, interest);
	accumulation = 0;

}
void SavingAccounts::show() {
	cout << "#ID" << id << "\tbalance: " << balance << endl;
	}
int main()
{
	//建立几个账户
	SavingAccounts s01(1,1234923,0.015), s02(1,2345982,0.015);
	//几笔账目
	s01.deposit(5, 5000);
	s02.deposit(15, 10000);
	s01.deposit(30, 5500);
	s02.withdraw(45, 5000);
	//开户后第90天到了银行结息日
	s01.settle(90);
	s02.settle(90);
	//输出各个账户信息
	s01.show(); cout << endl;
	s02.show(); cout << endl;
	 
    return 0;
}

