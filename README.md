# Ejemplo: Obteniendo la información sobre horarios de restaurantes en un radio de 500 metros con respecto a CEMFI.

```
library(googleway)
library(tidyverse)

key <- "MyPersonalAPIKey"
set_key(key = key)

res <- google_places(location = c(40.414273671703995, -3.689739986160891),
                    place_type = "restaurant",
                     radius = 500,
                     key = key)

restaurants.list <- data.frame(name = res$results$name, place_id = res$results$place_id)
restaurants.list


schedules <- list()
for (i in 1:nrow(restaurants.list)) {
  name <- print(restaurants.list$name[i])
  print(restaurants.list$place_id[i])
  place <- google_place_details(place_id = restaurants.list$place_id[i])
  schedules[[name]] <- place$result$opening_hours$weekday_text
}

schedules <- data.frame(schedules)
schedules <- data.frame(t(schedules))
schedules$names <- rownames(schedules)
rownames(schedules) <- c()

schedules <- schedules %>%
  select(names, everything())

names(schedules)[2:8] <- c("monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday")

schedules$monday <- substring(schedules$monday, 8, last = 1000000L)
schedules$tuesday <- substring(schedules$tuesday, 9, last = 1000000L)
schedules$wednesday <- substring(schedules$wednesday, 11, last = 1000000L)
schedules$thursday <- substring(schedules$thursday, 10, last = 1000000L)
schedules$friday <- substring(schedules$friday, 8, last = 1000000L)
schedules$saturday <- substring(schedules$saturday, 10, last = 1000000L)
schedules$sunday <- substring(schedules$sunday, 8, last = 1000000L)


library("writexl")
write_xlsx(schedules,"schedules.xlsx")
schedules
```

Resultado (16 primeras filas con datos disponibles):

| names | monday | tuesday | wednesday | thursday | friday | saturday | sunday |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| Trattoria.Sant.Arcangelo |  1:00 – 11:00 PM |  1:00 – 11:00 PM |  1:00 – 11:00 PM |  1:00 – 11:00 PM |  1:00 – 11:00 PM |  1:00 – 11:00 PM |  1:00 – 11:00 PM |
| El.Botánico.Restaurante.Terraza |  12:30 – 11:00 PM |  12:30 – 11:00 PM |  12:30 – 11:00 PM |  12:30 – 11:00 PM |  12:30 – 11:00 PM |  12:30 – 11:00 PM |  12:30 – 11:00 PM |
| Restaurante.Viridiana |  Closed |  1:30 – 3:30 PM, 8:30 – 11:00 PM |  1:30 – 3:30 PM, 8:30 – 11:00 PM |  1:30 – 3:30 PM, 8:30 – 11:00 PM |  1:30 – 3:30 PM, 8:30 – 11:00 PM |  1:30 – 3:30 PM, 8:30 – 11:00 PM |  1:30 – 3:30 PM |
| La.Tapería.del.Prado |  8:00 AM – 12:00 AM |  8:00 AM – 12:00 AM |  8:00 AM – 12:00 AM |  8:00 AM – 12:00 AM |  8:00 AM – 12:00 AM |  10:00 AM – 12:30 AM |  10:00 AM – 12:30 AM |
| Burger.King |  11:30 AM – 12:00 AM |  11:30 AM – 12:00 AM |  11:30 AM – 12:00 AM |  11:30 AM – 12:00 AM |  11:30 AM – 1:30 AM |  11:30 AM – 1:30 AM |  11:30 AM – 12:00 AM |
| Starbucks |  7:00 AM – 9:00 PM |  7:00 AM – 9:00 PM |  7:00 AM – 9:00 PM |  7:00 AM – 9:00 PM |  7:00 AM – 9:00 PM |  7:00 AM – 9:00 PM |  7:00 AM – 9:00 PM |
| La.Cocina.de.Neptuno |  Closed |  12:00 – 4:30 PM, 8:30 – 11:00 PM |  12:00 – 4:30 PM, 8:30 – 11:00 PM |  12:00 – 4:30 PM, 8:30 – 11:00 PM |  12:00 – 4:30 PM, 8:30 – 11:00 PM |  12:00 – 4:30 PM, 8:30 – 11:00 PM |  12:00 – 4:30 PM, 8:30 – 11:00 PM |
| Horcher |  1:15 – 4:00 PM, 8:30 – 11:00 PM |  1:15 – 4:00 PM, 8:30 – 11:00 PM |  1:15 – 4:00 PM, 8:30 – 11:00 PM |  1:15 – 4:00 PM, 8:30 – 11:00 PM |  1:15 – 4:00 PM, 8:30 – 11:00 PM |  8:30 – 11:00 PM |  Closed |
| AZ.Restaurante |  7:00 AM – 5:00 PM |  7:00 AM – 5:00 PM |  7:00 AM – 5:00 PM |  7:00 AM – 5:00 PM |  7:00 AM – 5:00 PM |  Closed |  Closed |
| Restaurante.Japonés.SAORI |  Closed |  Closed |  1:30 – 3:30 PM, 8:30 – 10:30 PM |  1:30 – 3:30 PM, 8:30 – 10:30 PM |  1:30 – 3:30 PM, 8:30 – 10:30 PM |  1:30 – 3:30 PM, 8:30 – 10:30 PM |  1:30 – 3:30 PM, 8:30 – 10:30 PM |
| Expressio.Cafe |  Closed |  12:00 PM – 12:00 AM |  12:00 PM – 12:00 AM |  12:00 PM – 12:00 AM |  1:00 PM – 12:00 AM |  12:00 PM – 12:00 AM |  12:00 PM – 12:00 AM |
| Viena.Capellanes |  7:30 AM – 4:30 PM |  7:30 AM – 4:30 PM |  7:30 AM – 4:30 PM |  7:30 AM – 4:30 PM |  7:30 AM – 4:30 PM |  Closed |  Closed |
| Cafetería.Restaurante.Alfonso.XI |  6:30 AM – 11:00 PM |  6:30 AM – 11:00 PM |  6:30 AM – 11:00 PM |  6:30 AM – 11:00 PM |  6:30 AM – 11:00 PM |  10:00 AM – 5:00 PM |  Closed |
| Condumios.Taberna |  8:00 AM – 4:00 PM |  8:00 AM – 4:00 PM, 8:00 – 11:00 PM |  8:00 AM – 4:00 PM, 8:00 – 11:00 PM |  8:00 AM – 4:00 PM, 8:00 – 11:00 PM |  8:00 AM – 4:00 PM, 8:00 – 11:00 PM |  1:00 – 4:00 PM, 8:00 – 11:00 PM |  Closed |
| El.Fogón.Verde |  Closed |  Closed |  1:00 – 4:00 PM |  1:00 – 4:00 PM |  1:00 – 4:00 PM |  1:00 – 4:00 PM |  1:00 – 4:00 PM |
| Taberna.La.Carola |  8:00 AM – 11:00 PM |  8:00 AM – 11:00 PM |  8:00 AM – 11:00 PM |  8:00 AM – 11:00 PM |  8:00 AM – 11:00 PM |  8:00 AM – 11:00 PM |  8:00 AM – 11:00 PM |
