#factura #facturación #detalle_factura #SQL

#### ***Conceptos*** sobre [[bt_ar_bi_isu_yfac_invo_det (Detalle)]]:

[cargo_variable] = `cast(SUM(CASE WHEN det.clase_pos_doc IN ('ZEAHP' ,'ZEAFP' ,'ZEARES','ZEAVAL','ZACTIV', 'ZCV') THEN det.monto ELSE 0 END) as decimal(30,2)) AS cargo_variable,`

[monto] = `cast(sum(case when trim(det.clase_pos_doc) not in ('ACCINF', 'SYNCDD','ACCMNT') THEN det.MONTO  ELSE 0 END) as decimal(20,2)) as monto,`

[potencia_convenida] = `cast(SUM(CASE WHEN det.clase_pos_doc IN ('ZPOTCV') THEN det.monto ELSE 0 END) as decimal(30,2)) AS potencia_convenida,  `

[potencia_adquirida] = `cast(SUM(CASE WHEN det.clase_pos_doc IN ('ZPOTAD') THEN det.monto ELSE 0 END) as decimal(30,2)) AS potencia_adquirida,  `

[exceso_potencia] = `cast(SUM(CASE WHEN det.clase_pos_doc IN ('ZPOTEX') THEN det.monto ELSE 0 END) as decimal(30,2)) AS exceso_potencia,`





