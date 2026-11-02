---
name: seo-schema
description: Adiciona dados estruturados JSON-LD (LocalBusiness/subtipo, serviços, preços, telefone, endereço, coordenadas, horário e FAQ) para negócios locais. Use com /seo-schema.
---

# JSON-LD para negócio local

## Entrada
Colete do código ou pergunte ao usuário (nunca invente):
- Nome, tipo de negócio, telefone, endereço completo (rua, número, bairro,
  cidade, UF, CEP), coordenadas (opcional)
- Horário de funcionamento
- Serviços oferecidos e preços, se publicados na página
- Perguntas frequentes **que aparecem visíveis na página**
- Redes sociais e Google Business Profile (para `sameAs`)

## Tipo schema.org
Use o subtipo mais específico de `LocalBusiness`:
- Dentista → `Dentist`
- Advogado → `Attorney` (ou `LegalService`)
- Nutricionista/clínica → `MedicalClinic` ou `MedicalBusiness`
- Agência de turismo, passeios → `TravelAgency` (passeios em `TouristTrip`)
- Restaurante, café, bar → `Restaurant`, `CafeOrCoffeeShop`, `BarOrPub`
- Salão, barbearia → `HairSalon`, `BeautySalon`
- Academia → `ExerciseGym`
- Loja → `Store` ou subtipo (`ClothingStore`, `PetStore` etc.)
- Pousada, hotel → `LodgingBusiness` ou subtipo (`Hotel`, `BedAndBreakfast`)
- Outros → consulte schema.org; na dúvida, `LocalBusiness`

Negócio sem endereço de atendimento (atende no cliente ou online): omita
`streetAddress` e use `areaServed` com as cidades atendidas.

## Passos
1. Gere um bloco no layout global ou na home. Se o projeto tem os dados em
   arquivo de conteúdo/tradução, monte o JSON a partir dele em vez de repetir
   os textos: assim JSON-LD e página nunca divergem.
   ```html
   <script type="application/ld+json">
   {
     "@context": "https://schema.org",
     "@type": "Dentist",
     "@id": "https://dominio.com/#business",
     "name": "",
     "url": "https://dominio.com",
     "image": "https://dominio.com/og.jpg",
     "logo": "https://dominio.com/icon-512.png",
     "telephone": "+55-11-99999-9999",
     "priceRange": "$$",
     "address": {
       "@type": "PostalAddress",
       "streetAddress": "",
       "addressLocality": "",
       "addressRegion": "SP",
       "postalCode": "",
       "addressCountry": "BR"
     },
     "geo": { "@type": "GeoCoordinates", "latitude": 0, "longitude": 0 },
     "openingHoursSpecification": [
       {
         "@type": "OpeningHoursSpecification",
         "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
         "opens": "08:00",
         "closes": "18:00"
       }
     ],
     "areaServed": "",
     "sameAs": [],
     "makesOffer": [
       {
         "@type": "Offer",
         "price": "150.00",
         "priceCurrency": "BRL",
         "itemOffered": { "@type": "Service", "name": "" }
       }
     ]
   }
   </script>
   ```
   Sem preço publicado na página, omita `price`/`priceCurrency`.
2. Bloco `WebSite` ligado ao negócio, que ajuda o Google a mostrar o nome do
   site no resultado:
   ```json
   { "@context": "https://schema.org", "@type": "WebSite",
     "@id": "https://dominio.com/#website", "url": "https://dominio.com",
     "name": "", "inLanguage": "pt-BR",
     "publisher": { "@id": "https://dominio.com/#business" } }
   ```
3. FAQ em bloco separado, na página onde as perguntas estão visíveis:
   ```html
   <script type="application/ld+json">
   {
     "@context": "https://schema.org",
     "@type": "FAQPage",
     "mainEntity": [
       { "@type": "Question", "name": "", "acceptedAnswer": { "@type": "Answer", "text": "" } }
     ]
   }
   </script>
   ```
4. Os dados do JSON-LD precisam bater com o que está visível na página (nome,
   telefone, endereço, preço). Divergência pode ser ignorada ou penalizada pelo
   Google.
5. Site multilíngue: cada versão leva o JSON-LD no seu idioma, com o mesmo
   `@id`.

## Avisos
- Desde 2023 o Google exibe rich result de FAQ quase só para sites
  governamentais e de saúde de autoridade. O FAQPage ainda ajuda na
  compreensão por buscadores e LLMs, mas não prometa o snippet expandido.
- Não adicione `aggregateRating` ou `review` com avaliações próprias; o Google
  não aceita autoavaliação em LocalBusiness. Nunca invente avaliações.

## Verificação
- JSON válido (parse sem erro) em todos os blocos.
- Sugira validar em https://validator.schema.org e
  https://search.google.com/test/rich-results após publicar.

## Saída
Mostre o JSON-LD final e onde foi inserido.
