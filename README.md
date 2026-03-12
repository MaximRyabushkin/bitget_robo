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
Модель не апеллирует к концепции "умных денег" или действиям больших игроков. Напротив, она опирается на тривиальное предположение: рациональное поведение любого участника, которому необходимо выйти из крупной позиции.

Если вы накопили значительную позицию, вы не можете выйти одним рыночным ордером не сдвинув цену против себя. Оптимальное решение — выставить лимитные ордера и ждать когда агрессивные контрагенты придут к вам — "выходить в силу", "продавать в покупки" (современная терминология нейросетей 🤷‍♂️).

Механика выхода не зависит от масштаба. Она одинаково применима и к розничному трейдеру с чувствительной позицией, и к хедж-фонду при выходе из большого объема. 
Более того, не играет роли, кто торгует против нас. Нам это и не нужно знать.

Мы стремимся обнаружить признаки этого поведения при распределении объёма в свинге: фаза накопления и фаза распределения должны быть примерно сбалансированы по объёму — то есть кто-то вошёл, и затем вышел. Если накопленный объём значительно превышает объём в фазе распределения — полагаем, что позиция не была полностью закрыта. Разворота не ожидается.

## How It Works / Как это работает

**EN:**
For each ticker and so-called timeframe, the bot continuously monitors for swing extremums — peaks and troughs — on range bars.

When an extremum is detected, the bot identifies the origin of the preceding movement and analyzes the volume distribution across the swing from origin to target.

**Phase analysis:**
The swing is split into two phases at the bar of maximum volume:
- Accumulation: from origin to the volume peak
- Distribution: from the volume peak to target

The core condition: distribution volume must be at least 90% of accumulation volume (ratio ≥ 0.9). This is the "position was unwound" check. If accumulation significantly exceeds distribution — no signal.

**Distribution shape:**
We do not require a perfect bell curve. Markets are not normally distributed. Instead we test:
- Volume peak is not at the very edge of the swing (peak position 0.1–0.9)
- Maximum volume is not on the final bar (no climax)
- Skewness ≤ 1.2, normalized entropy ≤ 0.95

**Delta confirmation:**
In the distribution phase, aggressive buyers should be hitting offers — generating positive delta. A participant exiting via limit sells needs that buying pressure. So delta_distribution > 0 is added as a confirmation filter. The reverse is true for sell-side trade.

**Signal trigger:**
Three-bar construction at the extremum confirms the turn. Entry on the close of the signal bar. Maximum volume traded side (green offer/red bid) must match the price bar direction.

**RU:**
Для каждого тикера и таймфрейма (в условном понимании) бот непрерывно отслеживает экстремумы свингов — пики и донья — на рейндж-барах.

При обнаружении экстремума бот определяет начало предшествующего движения и анализирует распределение объёма по свингу от начала до целевого бара.

**Анализ фаз:**
Свинг делится на две фазы в точке максимального объёма:
- Накопление: от начала до пика объёма (бар, на который пришелся максимальный объем)
- Распределение: от пика объёма до целевого бара

Ключевое условие: объём распределения должен составлять не менее 90% объёма накопления (соотношение ≥ 0.9). Это и есть подтверждение, что позиция была закрыта. 
Если накопление значительно превышает распределение — сигнала нет.

**Форма распределения:**
Мы не требуем идеального колокола в понимании нормального распределения.
Вместо этого проверяем набор технических условий:
- Пик объёма не расположен на самом краю свинга (позиция пика 0.1–0.9)
- Максимальный объём не на последнем баре (нет кульминации)
- Асимметрия ≤ 1.2, нормализованная энтропия ≤ 0.95

**Подтверждение дельты:**
В фазе распределения длинной позиции ожидается, что агрессивные покупатели будут бить по офферам, генерируя положительную дельту. Участник, выходящий лимитными ордерами, 
поглощает давление покупателей. Условие delta_distribution > 0 используется как фильтр подтверждения. Обратная логика применяется к продажам.

**Триггер сигнала:**
Трёхбарная конструкция на экстремуме подтверждает разворот. Вход на закрытии сигнального бара. Проторговка максимально объема (на оффере/на биде) должна совпадать с направлением бара.
