# Budget-app

class Category:
    def __init__(self,name):
        self.name = name
        self.total = 0.0
        self.ledger = []
    
    def __repr__(self):
        s = f"{self.name:*^30}\n"
        acc = 0
        for item in self.ledger:
            s += f"{item['description']} {item['amount']:>{30-len(item['description'])}}\n" 
            acc +=item['amount']
        s+= f"Total {acc}"
        return s
                                      
    
    
    def  deposit(self,amount,description=" "):
        self.total += amount
        self.ledger.append({"amount": amount, "description": description})


    def withdraw(self,amount,description):
        can_withdraw = self.check_funds(amount)
        if can_withdraw:
            self.total -= amount
            self.ledger.append({"amount": -amount, "description": description})
        return can_withdraw
    
    
    def get_balance(self):
        return self.total
    
    def transfer(self, amount, instance,):
        can_transfer = self.check_funds(amount)
        if can_transfer:
            self.withdraw(amount, f"Transfer to {instance.name}")
            instance.deposit(amount, f"Transfer from {self.name}")
        return can_transfer
    def check_funds(self, amount):
        if amount > self.total:
            return False
        return True
            
def create_spend_chart(catergories):
    s = "Percentage spent by category\n"
    total = 0
    caty = {}
    for cat in catergories:
        cat_total = 0
        for item in cat.ledger:
            amount = item["amount"]
            if amount < 0:
                total += amount
                cat_total += abs(amount)
        caty[cat.name] = cat_total
        
    for key, val in caty.items():
        percent = abs((val/total)*100)
        caty[key] = percent
    #print(caty)
        
    
    for num in range(100, -1, -10):
        s += f"{str(num) + '|':>4}"
        for val in caty.values():          
            if val >= num:               
                s += f" o"
        s +="\n"
    L = len(caty.values()) 
    s += f"    {(L*3+1)*'-' }"
    i = 0
