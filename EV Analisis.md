#estructura_de_ventas 
Analisis  de  [[Estructura de Ventas]]
cliente 1050, plan 43


el GUC es un cliente cuando el máximo entre la pot convenida y la pot adquirida supera los 300 kw

##### **Conceptos**
GUMA_= Grandes Usuarios Mayores, con una potencia mínima demandada de 1 MW. Su contrato de abastecimiento debe cubrir al menos el 50% de su demanda prevista, y el resto lo compran en el Mercado Spot.
GUME = Gran Usuario Menor, con una demanda de potencia superior a 100 kW e inferior a 2 MW. Contratan la demanda de energía total leída.
GUDI = Grandes Usuarios de la Distribuidora, con una potencia mínima demandada de 300 kW.
GUPA (IMPORTANTE ya no esta vigente! No existe!)= Eran Grandes Usuarios Particulares, con una demanda de potencia superior a 50 kW e inferior a 100 kW. Contratan la demanda de energía total leída. 
MEM = Mercado Eléctrico Mayorista.
Nemo = Identificación del suministro: a través del código alfanumérico de 8 dígitos (Nemo) dispuesto por CAMMESA. Para su correcta identificación se ha limitado el campo para que consigne sólo 8 dígitos (no consignar el código numérico “NIS”).

gumes son planes 28 92 94 , 
gudi  varios planes sueltos.
57 = 'VILLAS'

planes 92 son peaje t2

Es importante porque hay que asignarle un código y en base a eso se hacen muchas declaraciones. 

`greatest(m.pot_l_pta, m.pot_l_fpta) as pot_adquirida ,`
`m.pot_c_pta as pot_convenida`

pot_l_pta as pot_adquirida, 
pot_c_pta as pot_convenida



