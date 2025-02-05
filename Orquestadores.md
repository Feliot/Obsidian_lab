#tablas_enre #enre #Cloudera_datalake 
Cargas de tablas en [[Cloudera - Datalake]]

1. ENRE_PERIODO_DIARIO

	Este orquestador ingesta todas las tablas regulatorias diarias y genera la nueva tabla de facturación ST129

2. ENRE_PERIODO_SEMANAL

	Este orquestador genera la tabla regulatoria STMORO

3. ENRE_PERIODO_MENSUAL

	Este orquestador ingesta todas las tablas regulatorias de SAP (multables y no multables) y genera todas las tablas mensuales. Entre ellas estan STC1, STC2, ST21, ST15

4. ENRE_PERIODO_SEMESTRAL

	Este orquestador genera todas las tablas semestrales. Entre ellas estan ST11, ST12, ST13, ST14, ST16, ST18, PL/PF, Factura Digital, S121230, S121231
	
	Depende de la ingesta del mensual; por lo que para antes se debe ejecutar el orquestador ENRE_PERIODO_MENSUAL.