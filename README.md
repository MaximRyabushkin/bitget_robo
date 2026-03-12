# bitget-trading-bot

**EN:** An async futures trading bot built around a single idea: 
volume distribution across swing phases reveals optimal exit behavior.

**RU:** Асинхронный торговый робот, построенный вокруг одной идеи:
распределение объёма по фазам ценового движения отражает оптимальное поведение участника при выходе из позиции.

---

## The Idea / Идея

**EN:**
Range bars fix price movement, not time. Each bar covers an identical price interval — making it a natural histogram bin. A price swing from origin to target becomes a statistical sample: a distribution of volume across equal price increments.

In essence, volume × price range = work, in the physical sense. Range bars normalize the "distance" component, so volume becomes directly comparable across bars — a clean measure of market effort per unit of price movement.

**RU:**
Рейндж-бар представляет собой фиксированный ценовой диапазон (время не участвует явным образом), — и по сути является бином гистограммы. А отдельный сегмент рыночного движения может рассматриваться как статистическая выборка: распределением объёма по равным ценовым шагам.

В качестве физической аналогии, объём × диапазон цены = работа. Рейндж-бары нормируют расстояние, поэтому объём становится сопоставимым между барами. Таким образом мы получаем меру рыночного усилия на единицу ценового движения.
