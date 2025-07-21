#DAX Measures Used in Powerbi Report. 
## 1. Average Income By Employement Type = CALCULATE(AVERAGE('loan loan_default'[Income]),ALLEXCEPT('loan loan_default','loan loan_default'[EmploymentType]))


####

##2. Average Loan by Age Group = 
 AVERAGEX(VALUES('loan loan_default'[Age Group]),
 AVERAGE('loan loan_default'[LoanAmount]))


####



 ##3. Default Rate By Employment Type = 
 var totalRecords = COUNTROWS(ALL('loan loan_default'))
 var defaultCases = COUNTROWS(FILTER('loan loan_default','loan loan_default'[Default]=1))
 RETURN

 CALCULATE(DIVIDE(defaultCases,totalRecords),ALLEXCEPT('loan loan_default','loan loan_default'[EmploymentType])) *100

####
 

 ##4.Default Rate by Year = 
 var totalLoans = CALCULATE(COUNTROWS('loan loan_default'),ALLEXCEPT('loan loan_default','loan loan_default'[Year]))

 var defaultLoan = CALCULATE(COUNTROWS(FILTER('loan loan_default','loan loan_default'[Default]=1)),ALLEXCEPT('loan loan_default','loan loan_default'[Year]))
 RETURN
 DIVIDE(defaultLoan,totalLoans)*100

####
 
##5. Loan Amount by Purpose = 
SUMX(FILTER('loan loan_default',NOT(ISBLANK('loan loan_default'[LoanAmount]))),'loan loan_default'[LoanAmount])


####


##6. Loan amout for home loan = CALCULATE(SUM('loan loan_default'[LoanAmount]),'loan loan_default'[LoanPurpose]="home") 

####

##7. Average Loan amt(High Credit) = 
AVERAGEX(FILTER('loan loan_default','loan loan_default'[Credit Score Category]="High"),'loan loan_default'[LoanAmount]) 

####

##8.Median by Credit Score = 
 MEDIANX('loan loan_default','loan loan_default'[LoanAmount])

 ####

 ##9.Total Loan (Middle Age Adults) = 
CALCULATE(SUM('loan loan_default'[LoanAmount]),FILTER('loan loan_default','loan loan_default'[Age Group]="Middle Age Adults"))

####

##10.Total Loans (Credit Category) = 
CALCULATE(
    SUM('loan loan_default'[LoanAmount]),
    FILTER(
        'loan loan_default',
        'loan loan_default'[Age Group] = "Adults"
    ),
    ALLEXCEPT(
        'loan loan_default',
        'loan loan_default'[Age],
        'loan loan_default'[Age Group],
        'loan loan_default'[CreditScore],
        'loan loan_default'[Credit Score Category] 
    )
)
####


##11.YOY Default Loan Change = 
 DIVIDE(CALCULATE(COUNTROWS(FILTER('loan loan_default','loan loan_default'[Default]=1)),'loan loan_default'[Year]=YEAR(MAX('loan loan_default'[Loan Date (DD/MM/YYYY)]))) -
 CALCULATE(COUNTROWS(FILTER('loan loan_default','loan loan_default'[Default]=1)),'loan loan_default'[Year]=YEAR(MAX('loan loan_default'[Loan Date (DD/MM/YYYY)]))-1)


 ,
 CALCULATE(COUNTROWS(FILTER('loan loan_default','loan loan_default'[Default]=1)),'loan loan_default'[Year]=YEAR(MAX('loan loan_default'[Loan Date (DD/MM/YYYY)]))-1),0) *100

 ####

 ##12.YOY Loan Amount Change = 
 DIVIDE(
    CALCULATE(SUM('loan loan_default'[LoanAmount]),'loan loan_default'[Year]=YEAR(MAX('loan loan_default'[Loan Date (DD/MM/YYYY)])))-
     CALCULATE(SUM('loan loan_default'[LoanAmount]),'loan loan_default'[Year]=YEAR(MAX('loan loan_default'[Loan Date (DD/MM/YYYY)]))-1)

    ,
    CALCULATE(SUM('loan loan_default'[LoanAmount]),'loan loan_default'[Year]=YEAR(MAX('loan loan_default'[Loan Date (DD/MM/YYYY)]))-1),0) *100


####
##13.YTD Loan Amount = 
 CALCULATE(SUM('loan loan_default'[LoanAmount]),DATESYTD('loan loan_default'[Loan Date (DD/MM/YYYY)]),ALLEXCEPT('loan loan_default','loan loan_default'[Credit Score Category],'loan loan_default'[MaritalStatus]))  




