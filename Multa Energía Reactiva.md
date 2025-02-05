#energia_reactiva #multa #calculo

La multa o recargo por bajo [[Factor de Potencia]]  varia si es un Cliente T1, T2 o T3.
T1= `SI(factor < 0,95;((0,95 - factor)*1,5); 0)`
T2=`SI(factor < 0,95;((0,95 - factor)*1,5); 0)`
T3=`SI(factor > 0,33;((factor - 0,33)*1,5); 0)`

[tipocliente] ( #tipo_cliente ) = ( T1 ,T2 ,T3)

PHI = `SI(O(E2="";E2=0);0;
`SI(T2="T3";`
`REDONDEAR((E2)/SUMA(G2:I2);2);`
`SI(F2=0;0;REDONDEAR(COS(ATAN(E2/F2));2))))`

si #energia_r = 0 entonces 0 .
si #tipo_cliente ="T3" entonces; ( #energia_r / suma(pico, resto, valle))
si #tipo_cliente = T1 o T2 ,coseno( arcotangente(  #energia_r/ #consumo ))

[% de aumento] = (  #aumento] ) = `=SI(`[tipocliente]`="T3";`
`SI(` #tangente_phi`>0,33;((` #tangente_phi`-0,33)*1,5);0);`
`SI( Y(` #coseno_phi`<0,95;` #coseno_phi`<> 0 ); ((0,95- ` #coseno_phi`)*1,5); 0))`

Multa E reactiva] #Multa_Energia_Reactiva= `=SI(T2="T3";`
`(O2 * W2);`
`SI(T2="T2"; ( (O2+M2)*W2);((N2+O2) *W2)))`

En la factura la #unidad_de_medida es #kvar o "Kvar"
A la potencia reactiva Q se le asignó la letra “**r**”. Aunque la unidad de la [potencia reactiva] es coherente con los voltamperios, la Comisión Internacional Electrotécnica (IEC) propuso el símbolo en minúsculas “**var**”.

[Multa ER]  #multa_er ( #Multa_Energia_Reactiva)) `= IF(`[tipocliente]`="T3", ((` #cargo_variable  si es peaje(( energia consumida * (el precio de t3)) `+ ` #cargo_pot_excedida `+` #cargo_pot_adquirida `+` #cargo_pot_contratada `) * `[% de aumento]` )), IF(`[tipocliente]`= "T2", ( (` #cargo_variable  si es peaje(( energia consumida * (el precio de t3))  `+` #cargo_fijo `+` #cargo_pot_excedida `+` #cargo_pot_adquirida `+` #cargo_pot_contratada `) * `[% de aumento]`),((` #cargo_variable `+` #cargo_fijo`) * `[% de aumento]`)))`
o
[Multa ER]  = `round((case when left(tarifa, 2) != 'T1' then ((mer.cargo_variable + mer.cargo_pot_excedida + mer.cargo_pot_adquirida + cargo_pot_contratada} + (case when left(tarifa, 2) = 'T2' then cargo_fijo else 0 end)) * aumento) + ( case when right(tarifa, 5) = 'PEAJE' then cargo_reactiva else 0 end) else ((mer.cargo_fijo + mer.cargo_variable) * aumento) end), 2) as multa_er`


