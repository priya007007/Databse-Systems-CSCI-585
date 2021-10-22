In the ER Diagram, we have the following entities: 

1) PATIENT INFORMATION
Patient contains  details for each patient. It also contains the patient payment details i.e  what patient has paid so far for all treatments taken in this clinic and what is yet to pay for by the patient for all the procedures the patient has done so far in this clinic.

2) PATIENT CUMALITIVE HISTORY
The medications they currently on and allergies are multivalued attributes. This is to keep a record of their cumulative history if needed.

3)PATIENT VISIT 
It records the treatment done by each patient. A patient can have only 1 treatment/procedure done per day.  Hence the patient id together with the date forms a primary key. Using this table we can find out the medical history of the patient in this clinic. 

For every visit, the bill has to be generated and details, if the patient has insurance then that also needs to be sent. So we have the total amount which can be null if it is a follow up which is usually free. The insurance related attributes are null if the patient doesnt have any insurance. 

The payments should be completed within end of the month.
To make it simpler, I have noted the doctor id for a specific treatment for a patient at a date in the PATIENT VISIT entity. If we need to see the doctor helping in which room entity, we can query it using the relavant attributes.

4)ROOM
There is this entity called room for every visit which holds the state and Room id. A state could be:contacted, scheduled, recently visited, up for next visit, dormant.

5)MEDICAL PROFFESIONAL
Medical Professional includes dental hygienists, regular dentists, periodontists, endodontists, orthodontists and dental surgeons,hygienists and doctors. Anyone who is not a front office/desk worker and who is provides some kind of medical service to the patients. Together they form doctors or medical professional and have a doctor id. Doctor type will list the type such as dental hygienists, regular dentists etc.  All these are non-nullable values. A doctor needs to treat at least 1 patient since he/she works at the clinic.

6)LICENSES
Each doctor has a license that needs to be tracked. It cannot be null since if a doctor exists, he/she needs to have a license.  A STATUS_OF_LICENSE can be expired or valid.

7)OFFICE STAFF
The non-doctors who are in charge of administrative work. They are the ones who contact patients to do the scheduling. A staff member can be assigned to couple of patients. This way they will be in charge of contacting their assigned patients for their scheduling purposes. Since there are so many departments, a staff member may not be assigned to some departments hence we use the zero or more notation over here. Since if we have any of the 6 entities office staff is in charge of, we will have non nullable FK since we need to have a office staff assigned to them.
A single office staff will be assigned for a zero or more records(6 entities), a single record can be overseen by only 1 office staff member.
STAFF_ID will be the FK to see who handled the particular transaction. 

8)MONTHLY LEASE
Each month has monthly lease that needs to be paid. Since it is paid at a single transaction, we keep track as paid or not paid using STATUS_OF_LEASE. Every single month has something called LEASE_ID that is unique identifier. Office staff members are assigned multiple months to keep track of.

9)STARTUP COST
Since initially there was the initial startup cost, we have (few) office staff members who are in charge of many of the items. For each items,we store some attributes. Everything from software to supplies will be included in this.

10)LOAN REPAYMENT OF BANK
We assume that here the business only borrows from 1 bank. Hence we need to formulate a way to repay that money monthly. Each month there will be one transaction of a certain amount that will be sent to the bank. We will store the amount and status in our table. Again staff members will be assigned to multiple months(if a staff member is assigned to a bank account, no other staff will oversee that account). To keep track of months, we have a primary key called LOAN_ID. It will record for each installment to be paid (that will be done monthly). If we have a LOAN_ID, there will be some loan left to be paid.      
We also will consider penality from previous late installments. Total amount left to pay the bank to finish the 300,000 $ loan is the attribute TOTAL_AMOUNT_AFTER_THIS_TRAN.It is a derived attribute :TOTAL_AMOUNT_BEFORE_THIS_TRAN minus AMOUNT_TO_BE_PAID_THIS_MONTH. We will assume the busines does not intend to take more loan, and if so decides we will have to check and see if any adjustments are required. INT_RATE is the interest rate, even if it is 0, it cannot be null. We will just say 0%. 
The STATUS_OF_THIS_TRANSACTION can be paid or unpaid.

11)SALARY
A office staff member could be in charge of multiple salaries (across months and people). SAL_MON_ID refers to the unique id that will be given for every salary. It does not use month or the person, instead it is a unique id. Hence even the same person will not have the same number for 2 different months.  A specific salary transaction will be overseen by only one office staff.  A salary can have 2 STATUS_OF_SALARY, paid or unpaid.

12)MONTHLY SUPPLY OPERATIONAL STUFF
we also have the Supplies that are restocked every month,monthly operating costs such as facilities cleaning, utilities etc. Each of them will be given a unique transaction number (even if it is the same item that is repeating again will be given different number)that will be maintained by the staff. We will also have the status to see if the item has been purchased or not using STATUS_OF_M_SUPPLY which can be puchased or not purchased for each row.

13)INSURANCE COMPANY
List of insurance providers the patients of the clinic has. A patient can have max 1 Insurer.
 
14)COVERAGE TYPE
A patient can have either no coverage or a single coverage type by 1 insurance company.

Note: To see the day's scedule, we can query the ROOM (and PATIENT VISIT if needed to see the doctor that time)entity using the DATE_OF_VISIT attribute.

To see the billable income by the days services, we can query the PATIENT VISIT table using the DATE_OF_VISIT  attribute and use the TOTAL_AMOUNT_PAID.

The billing details are stored as PATIENT VISIT entity which tells all payment details(patient ID and other details) for a particular treatment detail along with COVERAGE TYPE, PATIENT INFORMATION and INSURANCE COMPANY entities which provides information such as  insurance, patient,coverage type and company name information that can be used for billing insurance providers.

The patient visits are stored in PATIENT_VISIT entity.

Startup cost is a one time cost that should be taken into account for long term or for initial planning. 

To calculate monthly expendure, we have 4 entities SALARY,LOAN REPAYMENT OF BANK, MONTHLY SUPPLY OPERATIONAL STUFF and MONTHLY LEASE. These 4 can be queried to calculate monthly expenditure. To calculate monthly income, we can query PATIENT VISIT entity based on DATE_OF_VISIT & TOTAL_AMOUNT_PAID attribute.

Hence the total monthly income minus monthly expenditure will be the profit.