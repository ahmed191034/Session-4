# Session-4
Explored SQL joins (INNER, LEFT, RIGHT), subqueries, and data analysis, linking customer data, subscriptions, and usage details. Uploaded script to GitHub for version control.
set sql_safe_updates=0;
select * from customer;
select * from billing;
select * from  service_packages;
select * from  subscriptions;
select * from feedback; 
# inner join


# write a query to find all customer along with thier billing information

select c.customer_id,c.first_name,c.last_name,b.amount_due,b.due_date
from customer c
inner join billing b on c.customer_id=b.customer_id;

select c.customer_id,c.First_name,c.Last_name,sum(b.amount_due) as Total_Amount_Due
from customer c
inner join billing b on c.customer_id=b.customer_id group by c.customer_id; 


select ser.package_name, count(ser.package_name)  as Number_of_Subscription
from subscriptions sub
inner join service_packages ser on sub.package_id=ser.package_id 
group by ser.package_name,sub.package_id;



# Left Join

select c.customer_id,f.feedback_text
from customer c
left join feedback f on c.customer_id=f.customer_id;

# retrive all the customer and the packages names of any subscription  they might have
select * from  service_packages;
select * from  subscriptions;
select * from customer;


select c.customer_id,c.First_name,c.Last_name,ser.package_name,ser.service_type
from subscriptions sub 
left join  service_packages ser on ser.package_id=sub.package_id
left join customer c on c.customer_id=sub.customer_id order by c.customer_id asc;


# find out which customer has never given feed back by left join
select * from feedback; 
select * from customer;
select fed.customer_id,c.First_name,c.Last_name,fed.rating
from feedback fed
left join  customer c on fed.customer_id = c.customer_id
having rating =0 order by customer_id asc;


# Right JOIN
# Show all the feedback entries and  the full names of customer who give them ?

select * from feedback order by customer_id asc; 
select * from customer;
select c.customer_id,concat(c.First_name," ", c.Last_name) as Full_Name,f.feedback_text,f.rating
from feedback f
right join customer c on f.customer_id=c.customer_id;
#having f.feedback_text is Null;


#list all the customer including those without a linked service message ? 
select * from customer;
select * from service_usage;



# Multiple Joints
# Write a Query to list all the customer,thier subscription packages and usage data
select * from customer;
select * from service_usage;
select * from service_packages;

select distinct c.customer_id,concat(c.first_Name," ",c.last_name) as Full_Name, serp.service_type,
seru.data_used,serp.package_name from customer c
inner join service_usage seru on c.customer_id=seru.customer_id
left join service_packages serp on seru.usage_id=serp.package_id order by c.customer_id asc; 

#SUBQUERIES
select package_name,monthly_rate from service_packages
where monthly_rate=(Select max(monthly_rate) from service_packages);

# find the customer with the smallest total amount data used in service usages


select * from service_usage;
select customer_id,data_used from service_usage
where data_used=(select min(data_used) from service_usage);  


# identify the service_package with the lowest monthly rate
select * from service_packages;
select package_id, monthly_rate from   service_packages
where monthly_rate=(select min(monthly_rate) from service_packages)

