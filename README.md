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

## The Model / Модель

**EN:**
The model makes no assumptions about "smart money" or institutional actors. It describes something simpler: the rational behavior of any participant who needs to exit a large position.

If you have accumulated a significant position, you cannot exit with a single market order without moving price against yourself. The optimal solution is to place limit orders and let aggressive counterparties come to you — exit into strength, sell into buying pressure.

This mechanics is scale-invariant. It applies equally to a retail trader managing a large-for-them position and to a hedge fund unwinding a block. Besides, we do not know who is on the other side. We do not need to.

What we can observe is the footprint of this behavior in the volume distribution: a swing where the accumulation phase and the distribution phase are roughly balanced in volume — meaning someone entered, and then exited. If accumulation volume significantly exceeds distribution, the position was not fully unwound. No reversal expected.

**RU:**
Модель не апеллирует к концепции "умных денег" или действиям больших игроков. Напротив, она опирается на общий факт: рациональное поведение любого участника, которому необходимо выйти из крупной позиции.

Если вы накопили значительную позицию, вы не можете выйти одним рыночным ордером не сдвинув цену против себя. Оптимальное решение — выставить лимитные ордера и ждать когда агрессивные контрагенты придут к вам — "выходить в силу", "продавать в покупки" (современная терминология нейросетей - прим. мое).

Эта механика не зависит от масштаба. Она одинаково применима и к розничному трейдеру с чувствительной позицией, и к хедж-фонду при выходе из большого объема. 
Более того, не играет роли, кто торгует против нас. Нам это и не нужно знать.

Мы стремимся обнаружить признаки этого поведения при распределении объёма в свинге: фаза накопления и фаза распределения должны быть примерно сбалансированы по объёму — то есть кто-то вошёл, и затем вышел. Если накопленный объём значительно превышает объём в фазе распределения — полагаем, что позиция не была полностью закрыта. Разворота не ожидается.
