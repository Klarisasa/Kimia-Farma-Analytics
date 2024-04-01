# Kimia-Farma-Analytics
select 
ft.transaction_id, 
ft.date,
ft.branch_id,
kc.branch_name,
kc.kota,
kc.provinsi,
kc.rating as rating_cabang,
ft.customer_name,
ft.product_id,
p.product_name,
p.price as actual_price,
ft.discount_percentage,
case  
  when p.price <= 50000 then p.price * 0.1
  when p.price <= 100000 then p.price * 0.15
  when p.price <= 300000 then p.price * 0.2
  when p.price <= 500000 then p.price * 0.25
  else p.price * 0.3
end as persentase_gross_laba,
(p.price-(p.price*ft.discount_percentage/100)) as nett_sales,
(p.price-(p.price*ft.discount_percentage/100))-(case  
  when p.price <= 50000 then p.price * 0.1
  when p.price <= 100000 then p.price * 0.15
  when p.price <= 300000 then p.price * 0.2
  when p.price <= 500000 then p.price * 0.25
  else p.price * 0.3
end) 
as nett_profit,
ft.rating as rating_transaksi
from kimia_farma.kf_final_transaction ft
left join kimia_farma.kf_product p
on ft.product_id=p.product_id
left join kimia_farma.kf_inventory i
on ft.branch_id=i.branch_id and ft.product_id=i.product_id
left join kimia_farma.kf_kantor_cabang kc
on ft.branch_id=kc.branch_id;

