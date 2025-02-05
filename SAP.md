#sap_isu #FICA #operando 

#Documento: Define el movimiento del Cliente.

#FICA: Financial Accounting Contract Accounts Receivable and Payable (Contrato de contabilidad financiera Cuentas por cobrar y por pagar.).
El módulo FICA contiene toda la información contable de los clientes, generadas a partir de transacciones de facturación, de pagos, de cálculo de intereses, devoluciones, es decir, todas las tareas relacionadas con la recaudación y reclamación.

#Operando: Contiene información con rango de vigencia desde-hasta, relevante para realizar determinadas acciones especialmente dentro de las lógicas de Cálculo. Se pueden definir Operandos a nivel de Tipo de Tarifa o Instalación.

#Unidad_lectura : Ruta de Lectura en calle.

#FacturaSD : Son documentos de facturación relacionados con un pedido de venta. 
	No están en ISU. Se crean de la siguiente manera:
	- Si hay una entrega en proceso, se crea un documento de facturación que hace referencia al documento de entrega.
	- Si hay productos de servicio sin una entrega, el documento de facturación se crea con referencia al pedido de venta

**Comandos**:
FPP3 = Interlocutor comercial.
CAA2 = Modificar Cuenta contrato.
FPE2 = Modificar Documento.
FPE3 = Visualizar Documento.
E41F = Ver Porción y Unidad de Lectura.
E41F = Ver Porción y Unidad de Lectura.
 