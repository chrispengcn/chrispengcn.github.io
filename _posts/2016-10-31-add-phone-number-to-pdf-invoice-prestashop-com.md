---
ID: 558
post_title: 'Add Phone Number to PDF Invoice &#8211; prestashop.com'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2016/10/31/add-phone-number-to-pdf-invoice-prestashop-com/
published: true
post_date: 2016-10-31 09:31:10
---
<p>&nbsp; <p><a title="https://www.prestashop.com/forums/topic/468694-transporteur-et-numeros-de-telephone-manquants-dans-facture-v161/page-2#entry2235421" href="https://www.prestashop.com/forums/topic/468694-transporteur-et-numeros-de-telephone-manquants-dans-facture-v161/page-2#entry2235421">https://www.prestashop.com/forums/topic/468694-transporteur-et-numeros-de-telephone-manquants-dans-facture-v161/page-2#entry2235421</a> <p>&nbsp; <p>&nbsp; <p>Juste pour info, pour ceux qui désirent avoir les n° de téléphones client et N° de TVA, s'ils existent, sur les factures/bons de livraison: <ul> <li>Vous trouverez 2 nouvelles lignes dans la table ps_configuration : PS_INVCE_INVOICE_ADDR_RULES et PS_INVCE_DELIVERY_ADDR_RULES qui déterminent quels sont les champs à ne pas afficher sur les factures/bons de livraison.  <li>Ces lignes comportent la valeur suivante: {"avoid":["vat_number","phone","phone_mobile"]} <li>Videz entièrement ces 2 tableaux comme ceci: {"avoid":[]} </li></ul> <p>Ces nouveaux paramètres ont été introduits (et inscrits en base de données lors de l'install/maj) depuis la 1.6.1.1 mais Prestashop a juste oublié de laisser la possibilité de les configurer...</p>