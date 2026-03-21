
# Do Fundamentals Matter? Testing Whether Financial Quality Predicts Stock Performance in the S&P 500
 
## ✅ Data Pipeline
 
- [x] Financial Data
- [x] Price Data (daily adjusted close)
- [x] S&P 500 Historical Components
 
---
 
## Quality Score Calculation
 
### Profitability
- [x] GPOA
- [x] ROE
- [x] ROA
- [x] GMAR
- [x] ACC
- [ ] CFOA
 
**Status: 5/6 (CFOA not implementing)**
 
### Safety
- [x] LEV (Leverage)
- [x] EVOL (Stability)
- [ ] BAB (Beta)
- [ ] O-Score (Ohlson)
- [ ] Z-Score (Altman)
 
**Status: 2/5 (BAB, O-Score, Z-Score not implementing)**
 
### Growth
- [x] ΔGPOA
- [x] ΔROE
- [x] ΔROA
- [x] ΔGMAR
- [ ] ΔCFOA
 
**Status: 4/5 (ΔCFOA not implementing)**


##  Backtesting Framework
 
### Core Backtesting Engine
 
- [x] Portfolio Construction & Rebalancing
- [x] Return & NAV Calculation
- [x] Trading Fees & Slippage
 
---
 
### ✅ Risk Metrics
 
- [x] CAGR, Volatility, Sharpe, Sortino, Calmar, MDD, Win Rate
 
---
 
### ✅ Strategy Implementations
 
- [x] Equal Weight
- [x] Rank-Based
- [x] Top3
- [x] Top3Rank
- [x] Top50
- [x] AbsMom (Absolute Momentum)
- [x] DualMom (Dual Momentum)
- [x] InvVol (Inverse Volatility)
- [x] AAA (All-Around Amazing)
 
---

### ✅ Hyperparameter Backtest
 
- [x] Support Multi Strategies
- [x] Support Multi Lookbacks
- [x] Support Multi Rebalances
 
---
 
## Introduction

ปัจจัยพื้นฐานยังคงเป็นกุญแจสำคัญในการสร้างผลตอบแทนจริงหรือไม่? การค้นหาคำตอบนี้เริ่มต้นขึ้นผ่านกลยุทธ์ Quality Minus Junk (QMJ) ในตลาดหุ้นสหรัฐฯ (S&P 500) โดยใช้ระบบการลงทุนเชิงปริมาณคัดเกรดหุ้นด้วยข้อมูลงบการเงินย้อนหลัง พร้อมทดสอบผ่านแบบจำลองการลงทุน (Backtest)เพื่อทดสอบความสม่ำเสมอของพอร์ตโฟลิโอหุ้นคุณภาพ และประเมินว่า fundamental quality สามารถอธิบายผลตอบแทนระยะยาวได้หรือไม่


## Data Source

ชุดข้อมูลพื้นฐาน (Fundamental Data) ประกอบด้วยข้อมูลจากงบกำไรขาดทุน (Income Statement), งบแสดงฐานะการเงิน (Balance Sheet) และงบกระแสเงินสด (Cash Flow) ของบริษัทจดทะเบียนในสหรัฐอเมริกา (US Listed Companies) ควบคู่กับข้อมูลราคาหุ้นรายวัน (Daily Price Data) และข้อมูล S&P 500 Historical Components & Changes

## Data Filtering & Processing

เพื่อให้ผลการทดสอบปราศจากอคติจากการเลือกเฉพาะบริษัทที่รอดชีวิตมาจนถึงปัจจุบัน (Survivorship Bias) เราใช้ข้อมูล S&P 500 Historical Components & Changes เพื่อระบุรายชื่อสมาชิกดัชนีตามจริงในแต่ละช่วงเวลา (Point-in-Time) ซึ่งเป็นหัวใจสำคัญในการกำหนดขอบเขตหุ้นที่ใช้ทดสอบ (Investment Universe) ให้ถูกต้องตามเหตุการณ์ที่เกิดขึ้นจริงในอดีต โดยหุ้นแต่ละตัวจะถูกนำมาวิเคราะห์เฉพาะในช่วงเวลาที่มีสถานะเป็นสมาชิกของดัชนีเท่านั้น

ข้อมูลดิบทั้งหมดจะผ่านกระบวนการ Data Cleaning เพื่อกำจัด Outliers และจัดการกับ Missing Data ก่อนจะเข้าสู่ขั้นตอน Feature Engineering เพื่อคำนวณอัตราส่วนทางการเงินเชิงคุณภาพสำหรับกลยุทธ์ QMJ ต่อไป

## Quality Scoring Methodology

การลงทุนแบบ QMJ (Quality Minus Junk) อาศัยการประเมินมูลค่าหุ้นตามทฤษฎีที่ว่า หุ้นที่มีมูลค่าสูงคือหุ้นที่ทำกำไรได้ดี มีความเสี่ยงต่ำ และมีการเติบโตอย่างต่อเนื่อง โดยมุ่งเน้นการใช้ข้อมูลจากงบการเงินเพื่อคำนวณหา "คุณภาพ (Quality)" ที่แท้จริงของบริษัทผ่านตัวเลขทางบัญชี เพื่อให้สามารถเปรียบเทียบคุณภาพของหุ้นที่มีขนาดธุรกิจและอุตสาหกรรมที่แตกต่างกันได้อย่างครอบคลุม เราจึงแบ่งมิติการวัดผลออกเป็น 3 เสาหลัก (Three Pillars of Quality) ดังนี้:

### Profitability (ความสามารถในการทำกำไร)

คะแนนส่วนนี้ใช้วัดความสามารถของบริษัทในการสร้างผลกำไรจากสินทรัพย์และส่วนของผู้ถือหุ้นที่มีอยู่ บริษัทที่มีคุณภาพจะต้องมีอัตรากำไรที่สูงสม่ำเสมอ และที่สำคัญคือผลกำไรนั้นควรมาจาก "เงินสด" จริง ไม่ใช่เพียงตัวเลขทางบัญชีที่เกิดจากรายการค้างรับค้างจ่าย (Accruals)

**วิธีการคำนวณ:** 
นำอัตราส่วนทางการเงินด้านล่างนี้มาแปลงเป็นค่ามาตรฐาน (Z-Score) แล้วหาค่าเฉลี่ย:

$$\text{Profitability} = z\left(\frac{1}{6}(z_{\text{GPOA}} + z_{\text{ROE}} + z_{\text{ROA}} + z_{\text{CFOA}} + z_{\text{GMAR}} + z_{\text{ACC}})\right)$$

| ตัวชี้วัด | สูตร | ความหมาย |
|----------|------|---------|
| **GPOA** | $\frac{\text{Revenue} - \text{COGS}}{\text{Total Assets}}$ | กำไรขั้นต้น / สินทรัพย์รวม เพื่อวัดประสิทธิภาพการใช้สินทรัพย์ในการสร้างกำไร |
| **ROE** | $\frac{\text{Net Income}}{\text{Book Equity}}$ | กำไรสุทธิ / ส่วนของผู้ถือหุ้น เพื่อวัดผลตอบแทนต่อเงินทุนของเจ้าของ |
| **ROA** | $\frac{\text{Net Income}}{\text{Total Assets}}$ | กำไรสุทธิ / สินทรัพย์รวม เพื่อวัดผลตอบแทนต่อสินทรัพย์ทั้งหมดของบริษัท |
| **CFOA** | $\frac{\text{Operating Cash Flow}}{\text{Total Assets}}$ | **กระแสเงินสดจากปฏิบัติการ / สินทรัพย์รวม** เพื่อวัดว่า "ผลกำไรมาจากเงินสดจริงเท่าไหร่"** ซึ่งเป็นตัวชี้วัดที่สำคัญที่สุดในการระบุบริษัทที่มีกำไรแท้จริง |
| **GMAR** | $\frac{\text{Revenue} - \text{COGS}}{\text{Revenue}}$ | กำไรขั้นต้น / ยอดขาย เพื่อวัดอัตรากำไรขั้นต้นจากการดำเนินงาน |
| **ACC** | $\frac{\text{Depreciation} - \Delta \text{Working Capital}}{\text{Total Assets}}$ | (ค่าเสื่อมราคา - การเปลี่ยนแปลงเงินทุนหมุนเวียน) / สินทรัพย์รวม โดยค่านี้ยิ่งต่ำยิ่งดี เพราะสะท้อนว่ากำไรที่เกิดขึ้นมาจากกระแสเงินสดจริงเป็นหลัก |
 

![Profitability Score](assets/profitability_A_diverging.png)
*Figure 1: ค่าเฉลี่ย z-score ของตัวชี้วัดด้าน Profitability (ROA, GPOA, ROE, GMAR และ Accruals) แสดงให้เห็นว่า Quality มีคะแนนสูงกว่า Junk อย่างชัดเจนในทุก metric หลัก โดยเฉพาะ ROA และ GPOA*

Quality portfolio มีความสามารถในการทำกำไรสูงกว่าอย่างมีนัยสำคัญในทุกตัวชี้วัดหลัก โดย ROA และ GPOA มีความแตกต่างเด่นชัดที่สุด ขณะที่ Accruals ไม่สามารถแยกกลุ่มได้อย่างมีประสิทธิภาพ สะท้อนว่าระดับของผลกำไรเป็นปัจจัยหลักในการกำหนดคุณภาพของบริษัทในโมเดลนี้

![Profitability Score](assets/profitability_B_spread.png)
*Figure 2: Q–J Spread ของตัวชี้วัดด้าน Profitability แสดงให้เห็นว่า ROA และ GPOA มี discriminatory power สูงที่สุด รองลงมาคือ ROE และ GMAR ขณะที่ Accruals มีบทบาทต่ำ*

การจัดอันดับแสดงให้เห็นว่า ROA เป็นตัวแปรที่มีความสามารถในการแยกหุ้นคุณภาพสูงสุด รองลงมาคือ GPOA ซึ่งทั้งสองสะท้อนถึงประสิทธิภาพในการใช้สินทรัพย์เพื่อสร้างกำไร ขณะที่ Accruals มีค่าใกล้ศูนย์และไม่สามารถแยกกลุ่มได้อย่างชัดเจน จึงมีบทบาทรองในโมเดล

![Profitability Score](assets/profitability_C_peryear.png)
*Figure 3: แนวโน้มคะแนน Profitability ของ Quality, Middle และ Junk ในแต่ละปี พร้อมค่า Q–J Spread แสดงให้เห็นว่า Quality มีคะแนนสูงกว่า Junk อย่างสม่ำเสมอ และ spread ยังคงเป็นบวกในทุกช่วงเวลา*

ผลลัพธ์สะท้อนว่า Profitability factor มีความสม่ำเสมอในเชิงเวลา โดย Quality portfolio มีระดับผลกำไรสูงกว่า Junk อย่างต่อเนื่องในทุกปี และ Q–J Spread ยังคงเป็นบวกตลอดช่วงเวลา แม้จะมีความผันผวนในบางช่วง เช่น 2012–2015 ที่ spread อ่อนตัวลงจากภาวะตลาดหรือวัฏจักรเศรษฐกิจ อย่างไรก็ตาม แนวโน้มโดยรวมยังยืนยันว่าความสามารถในการทำกำไรเป็นสัญญาณหลักที่มีเสถียรภาพในการแยกหุ้นคุณภาพในระยะยาว

### Safety (ความปลอดภัยทางการเงิน)

คะแนนส่วนนี้ใช้วัดระดับ "ความเสี่ยง" ของบริษัท หุ้นที่มีคุณภาพสูงจะต้องมีความทนทานต่อสภาวะเศรษฐกิจตกต่ำ (Low Risk) มีภาระหนี้สินในระดับที่ควบคุมได้ และมีผลประกอบการในอดีตที่สม่ำเสมอ ไม่มีความผันผวนที่รุนแรงจนเกินไป

**วิธีการคำนวณ:** คำนวณจากการนำอัตราส่วนด้านล่างมาทำเป็นค่ามาตรฐาน (Z-Score) โดยปรับเกณฑ์ให้บริษัทที่มีความเสี่ยงต่ำได้รับคะแนนสูง:

$$\text{Safety} = z\left(\frac{1}{5}(z_{\text{BAB}} + z_{\text{LEV}} + z_{\text{O-Score}} + z_{\text{Z-Score}} + z_{\text{EVOL}})\right)$$


| ตัวชี้วัด | สูตร | ความหมาย |
|----------|------|---------|
| **BAB** (Beta) | $-\beta_{\text{market}}$ | **Market Beta ต่ำ** = ความไวต่อความผันผวนตลาดต่ำ เพื่อวัดความสัมพันธ์กับตลาดโดยรวม (β<1 = ป้องกันตัวได้ดี) |
| **LEV** (Leverage) | $-\frac{\text{Total Debt}}{\text{Total Assets}}$ | **หนี้สินรวม / สินทรัพย์รวม** ยิ่งต่ำยิ่งดี เพราะสะท้อนถึงภาระหนี้ที่น้อยและความเสี่ยงในการผิดนัดชำระหนี้ที่ต่ำกว่า |
| **O-Score** (Ohlson) | Ohlson Bankruptcy Model | **ความเสี่ยงการล้มละลาย** (Ohlson O-Score) ต่ำ เป็นแบบจำลองทางสถิติเพื่อคาดการณ์ความน่าจะเป็นล้มละลาย |
| **Z-Score** (Altman) | Altman Z-Score | **ความเสี่ยงการล้มละลาย** (Altman Z-Score) ต่ำ ใช้วัดความเสี่ยงด้านการล้มละลายจากหลายมิติเช่น โครงสร้างสินทรัพย์ กำไร และประวัติการดำเนินงาน |
| **EVOL** (Stability) | $\text{StdDev}(\text{ROE, past 60 qtr})$ | **ความผันผวนของกำไร** (ส่วนเบี่ยงเบนมาตรฐาน) ยิ่งต่ำยิ่งดี เพราะสะท้อนว่า บริษัทมีผลประกอบการที่เสถียรและคาดการณ์ได้ |
 

![Safety Score](assets/safety_A_diverging.png)
*Figure 4: ค่าเฉลี่ย z-score ของตัวชี้วัดด้าน Safety (Leverage และ Stability) แสดงให้เห็นว่า Quality มีคะแนนสูงกว่า Junk อย่างชัดเจน โดยมี gap สูงในทั้งสองมิติ โดยเฉพาะ Leverage ซึ่งมีความแตกต่างมากที่สุด*

Quality portfolio มีลักษณะความเสี่ยงที่ดีกว่าอย่างมีนัยสำคัญ โดยมีระดับหนี้สินต่ำกว่า (Leverage) และความผันผวนของกำไรต่ำกว่า (Stability) เมื่อเทียบกับ Junk ซึ่งสะท้อนถึงความแข็งแกร่งของโครงสร้างทางการเงิน นอกจากนี้ความแตกต่างที่สูงในมิติ Leverage บ่งชี้ว่าการควบคุมภาระหนี้เป็นปัจจัยสำคัญในการแยกหุ้นคุณภาพในโมเดลนี้

![Safety Score](assets/safety_B_spread.png)
*Figure 5: Q–J Spread ของตัวชี้วัดด้าน Safety แสดงให้เห็นว่า Leverage มีความสามารถในการแยกกลุ่มสูงที่สุด รองลงมาคือ Stability โดยทั้งสองปัจจัยให้สัญญาณเชิงบวกอย่างชัดเจน*

Leverage เป็นตัวแปรหลักในการแยกหุ้นคุณภาพ โดยมีค่า Q–J Spread สูงที่สุดอย่างมีนัยสำคัญ สะท้อนว่าบริษัทที่มีภาระหนี้ต่ำมีแนวโน้มถูกจัดอยู่ในกลุ่ม Quality มากกว่า ขณะที่ Stability แม้มีบทบาทรองลงมา แต่ยังช่วยเสริมความสามารถของโมเดลในการระบุบริษัทที่มีผลประกอบการสม่ำเสมอ ซึ่งทั้งสองปัจจัยร่วมกันสร้างมิติด้านความเสี่ยงที่สำคัญของ Quality factor

![Safety Score](assets/safety_C_peryear.png)
*Figure 6: แนวโน้มคะแนน Safety (Leverage และ Stability รวม) ของ Quality, Middle และ Junk ในแต่ละปี พร้อมค่า Q–J Spread แสดงให้เห็นว่า Quality มีคะแนนสูงกว่า Junk อย่างสม่ำเสมอ และ spread ยังคงเป็นบวกในทุกช่วงเวลา*

Safety factor มีความสม่ำเสมอในเชิงเวลา โดย Quality portfolio มีคะแนนสูงกว่า Junk อย่างต่อเนื่องในทุกปี ขณะที่ Q–J Spread อยู่ในระดับบวกตลอดช่วงเวลา แม้จะมีความผันผวนในบางปี ซึ่งสะท้อนถึงความเสถียรของสัญญาณด้านความเสี่ยงในระยะยาว และยืนยันว่าปัจจัยด้านโครงสร้างทางการเงินมีบทบาทสำคัญในการแยกหุ้นคุณภาพอย่างต่อเนื่อง

### Growth (การเติบโต)

นอกจากการทำกำไรได้ดีและมีความปลอดภัยแล้ว บริษัทที่มีคุณภาพจะต้องแสดงให้เห็นถึง "การเติบโต" ของผลประกอบการอย่างต่อเนื่อง คะแนนส่วนนี้จึงถูกนำมาใช้เพื่อแยกแยะระหว่างบริษัทที่กำลังขยายตัว (Expanding) ออกจากบริษัทที่เริ่มอิ่มตัวหรืออยู่ในภาวะถดถอย

**วิธีการคำนวณ:** คำนวณจากการเปลี่ยนแปลง (Delta หรือ Δ) ของอัตราส่วนในกลุ่ม Profitability โดยเปรียบเทียบปีปัจจุบันกับข้อมูลย้อนหลัง 3 ปี (หรือตามระยะเวลาที่กำหนด) จากนั้นนำมาทำ Z-Score:

$$\text{Growth} = z\left(\frac{1}{5}(z_{\Delta \text{GPOA}} + z_{\Delta \text{ROE}} + z_{\Delta \text{ROA}} + z_{\Delta \text{CFOA}} + z_{\Delta \text{GMAR}})\right)$$

| ตัวชี้วัด | สูตร | ความหมาย |
|----------|------|---------|
| **ΔGPOA** | $\frac{\text{GPOA}_{t} - \text{GPOA}_{t-5}}{\text{GPOA}_{t-5}}$ | การเติบโตของ Gross Profit Margin โดยวัดจากการเปลี่ยนแปลงใน 5 ปี |
| **ΔROE** | $\frac{\text{ROE}_{t} - \text{ROE}_{t-5}}{\text{ROE}_{t-5}}$ | การเติบโตของผลตอบแทนแนนบนแนบนแนบนแนบน แสดงให้เห็นว่าเจ้าของเกิดประโยชน์มากขึ้นหรือน้อยลง |
| **ΔROA** | $\frac{\text{ROA}_{t} - \text{ROA}_{t-5}}{\text{ROA}_{t-5}}$ | การเติบโตของผลตอบแทนสินทรัพย์ แสดงว่าบริษัทใช้สินทรัพย์อย่างมีประสิทธิภาพมากขึ้นหรือไม่ |
| **ΔCFOA** | $\frac{\text{CFOA}_{t} - \text{CFOA}_{t-5}}{\text{CFOA}_{t-5}}$ | การเติบโตของกระแสเงินสด แสดงว่าบริษัทสร้างเงินสดจริงได้มากขึ้นหรือน้อยลง ซึ่งสำคัญมากสำหรับความยั่งยืน |
| **ΔGMAR** | $\frac{\text{GMAR}_{t} - \text{GMAR}_{t-5}}{\text{GMAR}_{t-5}}$ | การเติบโตของกำไรขั้นต้น แสดงว่าบริษัทสามารถรักษากำไรขั้นต้นไว้หรือเพิ่มขึ้น |

![growth Score](assets/growth_A_diverging.png)
*Figure 7: ค่าเฉลี่ย z-score ของตัวชี้วัดด้าน Growth (การเปลี่ยนแปลงระยะ 3 ปี) แสดงให้เห็นว่า Quality มีค่าการเติบโตสูงกว่า Junk อย่างสม่ำเสมอในทุก metric โดยมี Q–J Spread เป็นบวกในทุกตัวชี้วัด*

Quality portfolio ไม่เพียงมีผลประกอบการที่ดีในปัจจุบัน แต่ยังมีแนวโน้มการเติบโตที่แข็งแกร่งกว่าในระยะยาว โดยตัวชี้วัดการเปลี่ยนแปลง เช่น ΔROA และ ΔGPOA แสดงให้เห็นความแตกต่างที่ชัดเจนระหว่าง Quality และ Junk ขณะที่ค่าเฉลี่ยของ Middle อยู่ใกล้ศูนย์ สะท้อนว่า Growth มีบทบาทเป็นปัจจัยเสริม (secondary factor) มากกว่าตัวขับเคลื่อนหลัก เมื่อเทียบกับ Profitability และ Safety

![growth Score](assets/growth_B_spread.png)
*Figure 8: Q–J Spread ของตัวชี้วัดด้าน Growth แสดงให้เห็นว่า ΔROA มีความสามารถในการแยกกลุ่มสูงที่สุด รองลงมาคือ ΔGPOA, ΔGMAR และ ΔROE โดยทุกตัวชี้วัดให้สัญญาณเชิงบวกอย่างสม่ำเสมอ*

ΔROA เป็นตัวแปรที่มีพลังในการแยกหุ้นคุณภาพสูงที่สุดในกลุ่ม Growth สะท้อนถึงความสำคัญของการปรับตัวดีขึ้นของผลตอบแทนต่อสินทรัพย์ ขณะที่ตัวชี้วัดอื่น เช่น ΔGPOA และ ΔGMAR ยังมีบทบาทสนับสนุนในการจับการเติบโตของประสิทธิภาพการดำเนินงาน แม้ว่าโดยรวมแล้วมิติด้าน Growth จะมีอิทธิพลรองลงมาจาก Profitability และ Safety แต่ยังช่วยเสริมความสมบูรณ์ของโมเดลในการประเมินคุณภาพหุ้นในระยะยาว

![growth Score](assets/growth_C_peryear.png)
*Figure 9: แนวโน้มคะแนน Growth ของ Quality, Middle และ Junk ในแต่ละปี พร้อมค่า Q–J Spread แสดงให้เห็นว่า Quality มีคะแนนสูงกว่า Junk อย่างสม่ำเสมอ และ spread ยังคงเป็นบวกในทุกช่วงเวลา*

ผลลัพธ์สะท้อนว่า Growth factor มีความสม่ำเสมอในเชิงเวลา โดย Quality portfolio มีแนวโน้มการเติบโตที่สูงกว่า Junk อย่างต่อเนื่องในทุกปี แม้จะมีความผันผวนในบางช่วง ขณะที่ Q–J Spread ยังคงเป็นบวกตลอดช่วงเวลา ซึ่งยืนยันว่าความสามารถในการปรับตัวและพัฒนาในระยะยาวเป็นคุณลักษณะสำคัญของหุ้นคุณภาพ

### Composite Quality Score

เพื่อให้ได้ผลสรุปของคุณภาพหุ้นในภาพรวม ข้อมูลจากทั้ง 3 เสาหลักจะถูกนำมารวมผ่านกระบวนการดังนี้:

1. **Standardization:** ตัวชี้วัดย่อยทั้งหมดจะถูกแปลงเป็นค่ามาตรฐาน (Z-Score) เพื่อปรับหน่วยวัดที่แตกต่างกันให้สามารถนำมาคำนวณร่วมกันได้ โดยทิศทางของคะแนนจะถูกปรับให้หุ้นที่มีคุณภาพสูง (ความเสี่ยงต่ำ, กำไรสูง, เติบโตสูง) ได้รับคะแนนที่เป็นบวกมากที่สุด
   
$$z(x) = \frac{r_x - \mu_r}{\sigma_r}$$


2. **Aggregation:** คำนวณหาค่าเฉลี่ยของคะแนนในแต่ละมิติ (Profit Score, Safety Score, Growth Score) จากนั้นนำคะแนนทั้ง 3 ด้านมาหาค่าเฉลี่ยรวมอีกครั้งเพื่อให้ได้คะแนนคุณภาพสุทธิ หรือ Composite Quality Score:

$$\text{Quality Score}_{i,t} = z\left(\frac{1}{3}(\text{Profitability}_{i,t} + \text{Safety}_{i,t} + \text{Growth}_{i,t})\right)$$

โดยที่:
- $\text{Profitability}_{i,t}$ = คะแนน Profitability ของหุ้น $i$ ในช่วงเวลา $t$
- $\text{Safety}_{i,t}$ = คะแนน Safety ของหุ้น $i$ ในช่วงเวลา $t$
- $\text{Growth}_{i,t}$ = คะแนน Growth ของหุ้น $i$ ในช่วงเวลา $t$
- $z(\cdot)$ = ฟังก์ชัน standardization (z-score transformation) ที่นำเสนอด้านบน

3. **Ranking & Portfolio Construction:** หุ้นทั้งหมดในดัชนี S&P 500 จะถูกจัดลำดับตามคะแนนรวมที่ได้ โดยหุ้นที่มีคะแนนสูงสุด 30% แรกจะถูกคัดเลือกเข้าสู่พอร์ตโฟลิโอหุ้นคุณภาพ (Quality Portfolio) และหุ้นที่มีคะแนนต่ำสุด 30% หลังจะถูกจัดเป็นกลุ่มหุ้นด้อยค่า (Junk Portfolio)

$$\text{Percentile}_{i,t} = \frac{\text{Rank}(\text{QMJ Score}_i) - 1}{N - 1} \times 100\%$$

โดยที่:
- $N$ = จำนวนหลักทรัพย์ทั้งหมดในช่วงเวลา $t$
- Quality Portfolio = สต็อกที่ Percentile ≥ 70% (Top 30%)
- Junk Portfolio = สต็อกที่ Percentile ≤ 30% (Bottom 30%)


## QMJ Score Distribution & Portfolio Assignment
### QMJ Score Analysis 
![growth Score](assets/qmj_score_distribution.png)
*Figure 10: การกระจายตัวของ QMJ Score ในช่วง FY2023–2025 แสดงให้เห็นว่าคะแนนมีลักษณะใกล้เคียงการแจกแจงแบบปกติ (normal distribution) และมีการแบ่งกลุ่ม Quality (Top 30%) และ Junk (Bottom 30%) อย่างชัดเจนในทุกปี โดย threshold ของ Quality อยู่ในช่วงค่าบวก และ Junk อยู่ในช่วงค่าลบอย่างสม่ำเสมอ สะท้อนถึงความเสถียรของกระบวนการจัดอันดับ (ranking stability) และความสามารถของโมเดลในการแยกหุ้นคุณภาพสูงออกจากหุ้นคุณภาพต่ำได้อย่างต่อเนื่อง*

![growth Score](assets/qmj_components_boxplot.png)
*Figure 11: การกระจายตัวของคะแนน QMJ components (Profit, Safety, Growth) แสดงให้เห็นว่า Quality มี distribution ที่เลื่อนไปทางค่าบวก ขณะที่ Junk กระจายตัวในช่วงค่าลบอย่างสม่ำเสมอ สะท้อนถึงความสามารถของแต่ละปัจจัยในการแยกกลุ่มหุ้นได้ในเชิง cross-sectional*

Outliers ที่ปรากฏใน distribution ของ QMJ components เป็นลักษณะปกติของ financial data ซึ่งมักมีลักษณะ heavy-tailed จากความแตกต่างของโครงสร้างธุรกิจ (เช่น asset-light vs asset-heavy) และวัฏจักรเศรษฐกิจ ทำให้บางบริษัทมีค่าที่เบี่ยงจากกลุ่มอย่างชัดเจน นอกจากนี้ยังอาจเกิดจากรายการทางบัญชีหรือเหตุการณ์พิเศษ (one-off) เช่น การเปลี่ยนแปลงเงินทุนหมุนเวียนหรือกำไรพิเศษ ซึ่งส่งผลให้ค่าบางช่วงเวลาผันผวนสูงกว่าปกติ

![growth Score](assets/qmj_mean_component_bar.png)
*Figure 12: ค่าเฉลี่ยของคะแนน QMJ components ระหว่าง Quality และ Junk แสดงให้เห็นว่า Safety มี discriminatory power สูงที่สุด รองลงมาคือ Growth และ Profit ซึ่งยืนยันว่าทั้งสามมิติสามารถแยกหุ้นคุณภาพได้อย่างมีนัยสำคัญ*

![growth Score](assets/qmj_factor_correlation_heatmap.png)
*Figure 13: Correlation heatmap ของ QMJ factors (z-scores) แสดงความสัมพันธ์ระหว่างตัวชี้วัดในแต่ละมิติ โดยส่วนใหญ่มีความสัมพันธ์ในระดับต่ำถึงปานกลาง สะท้อนว่าแต่ละ factor ให้ข้อมูลที่แตกต่างกัน (diversified signals) และไม่ซ้ำซ้อนกันมากนัก ซึ่งช่วยเพิ่มประสิทธิภาพของ composite model ในการจับสัญญาณคุณภาพหุ้นได้ดียิ่งขึ้น*

![growth Score](assets/qmj_score_trend_with_spreads.png)
*Figure 14: QMJ Score และ Q–J Spread ของแต่ละ component ในช่วงเวลา*

QMJ Score และ component spreads มีความเสถียรในระยะยาว โดย Quality outperform Junk อย่างต่อเนื่องในทุกมิติ แม้มีความผันผวนระยะสั้นในบางช่วงเวลา

## Stock-Level Quality
![qmj_top_quality_junk_heatmap](assets/qmj_top_quality_junk_heatmap.png)
*Figure 15: Heatmap ของ QMJ Score สำหรับหุ้นที่ติดกลุ่ม Quality และ Junk บ่อยที่สุดในช่วงปี 2006–2025 โดยตัวเลขในแต่ละช่องแสดงค่า QMJ Score และ percentile rank ของหุ้นในปีนั้น ๆ ขณะที่คอลัมน์ Avg สรุปค่าเฉลี่ยตลอดช่วงเวลา ผลลัพธ์แสดงให้เห็นว่าหุ้นในกลุ่ม Quality มีคะแนนเป็นบวกและอยู่ใน percentile สูงอย่างสม่ำเสมอ ขณะที่หุ้นในกลุ่ม Junk มีคะแนนเป็นลบและอยู่ใน percentile ต่ำต่อเนื่อง สะท้อนถึงความคงทนของสัญญาณคุณภาพในระดับรายหลักทรัพย์ (stock-level persistence)*

## Sector-Level Quality Dynamics

![sector_scatter](assets/sector_scatter.png)
*Figure 16: การกระจายตัวของ sector บนแกน Profitability และ Safety โดยขนาดของจุดสะท้อน Growth score แสดงให้เห็นว่า Technology และ Healthcare อยู่ในโซน High Profit–High Safety อย่างชัดเจน ขณะที่ Utilities และ Real Estate อยู่ในโซนคุณภาพต่ำ โดยภาพรวมสะท้อนโครงสร้างคุณภาพของแต่ละอุตสาหกรรมได้อย่างชัดเจน*

แต่ละ sector มีตำแหน่งที่แตกต่างกันอย่างมีนัยสำคัญ โดย Technology และ Healthcare มีความสมดุลทั้งด้านกำไรและความปลอดภัย ขณะที่บาง sector เช่น Energy และ Financials มีลักษณะผันผวนตามวัฏจักรเศรษฐกิจ ซึ่งสะท้อนว่า Quality factor มีลักษณะเฉพาะในระดับอุตสาหกรรม

![sector_scatter](assets/sector_component_heatmap.png)
*Figure 17: ค่าเฉลี่ยของ Profitability, Safety และ Growth ในแต่ละ sector พร้อมลำดับอันดับภายในกลุ่ม แสดงให้เห็นถึงองค์ประกอบที่ขับเคลื่อนคุณภาพของแต่ละอุตสาหกรรม*

แต่ละ sector มี “driver” ที่แตกต่างกัน โดย Technology มีความแข็งแกร่งทั้งด้าน Profitability และ Safety ขณะที่บาง sector เช่น Energy มีคะแนน Safety สูงแต่ Profitability ต่ำ สะท้อนว่าโครงสร้างคุณภาพไม่ได้มาจากปัจจัยเดียว แต่เป็นการผสมผสานของหลายมิติ

![sector_qmj_heatmap](assets/sector_qmj_heatmap.png)
*Figure 18: ค่าเฉลี่ย QMJ Score ของแต่ละ sector ในแต่ละปี โดยกล่องสีเขียวแสดง sector ที่ดีที่สุด และสีแดงแสดง sector ที่แย่ที่สุดในแต่ละปี*

ผลลัพธ์สะท้อนให้เห็นว่า ผลการดำเนินงานของแต่ละ sector มีการเปลี่ยนแปลงตามช่วงเวลา โดยบางอุตสาหกรรม เช่น Energy มีมีลักษณะเป็นวัฏจักร (cyclical) ขณะที่ Technology และ Healthcare มีแนวโน้มให้คะแนนสูงอย่างสม่ำเสมอ ซึ่งแสดงถึงความแตกต่างของ regime ในแต่ละช่วงเศรษฐกิจ

![quality_pct_heatmap](assets/quality_pct_heatmap.png)
*Figure 19: สัดส่วนของหุ้นที่อยู่ในกลุ่ม Quality (Top 30%) ในแต่ละ sector ต่อปี โดยสัญลักษณ์ ▲ แสดงค่าที่สูงกว่า baseline*

บาง sector เช่น Technology และ Healthcare มีสัดส่วนหุ้นคุณภาพสูงอย่างต่อเนื่อง สะท้อนถึงความสามารถในการสร้างบริษัทที่มีคุณภาพในระดับโครงสร้าง ขณะที่บาง sector มีสัดส่วนที่ผันผวนมากกว่า ซึ่งบ่งชี้ถึงความไม่สม่ำเสมอของคุณภาพในกลุ่มนั้น

![junk_pct_heatmap](assets/junk_pct_heatmap.png)
*Figure 20: สัดส่วนของหุ้นที่อยู่ในกลุ่ม Junk (Bottom 30%) ในแต่ละ sector ต่อปี โดยสัญลักษณ์ ▲ แสดงค่าที่สูงกว่า baseline*

sector ที่มีสัดส่วน Junk สูงอย่างต่อเนื่อง เช่น Utilities และ Real Estate สะท้อนถึงข้อจำกัดเชิงโครงสร้างของอุตสาหกรรม ขณะที่ sector อื่นมีการเปลี่ยนแปลงตามวัฏจักร ซึ่งช่วยยืนยันภาพรวมของความแตกต่างด้านคุณภาพระหว่างกลุ่มอุตสาหกรรม

##  Backtesting Results
###  Portfolio Performance 
![backtest_equity_curve_crises](assets/backtest_equity_curve_crises.png)
*Figure 21: ผลการทดสอบพอร์ต Quality เทียบกับ Junk และ S&P 500 แสดงให้เห็นว่า Quality สร้างผลตอบแทนสะสมสูงกว่าอย่างมีนัยสำคัญ (CAGR 8.6% vs 5.7%) พร้อม Sharpe ratio ที่ดีกว่า และ drawdown ที่ตื้นกว่า ขณะที่ long-short (Quality − Junk) ให้ spread เป็นบวกอย่างต่อเนื่องในระยะยาว*

ผลลัพธ์จากการ backtest แสดงให้เห็นว่า Quality portfolio สามารถ outperform ทั้ง Junk และตลาดโดยรวมอย่างสม่ำเสมอในระยะยาว โดยมีทั้งผลตอบแทนที่สูงกว่าและความเสี่ยงที่ต่ำกว่า ขณะที่ long-short strategy ให้ spread เพิ่มขึ้นอย่างต่อเนื่องโดยไม่มีการกลับทิศเชิงโครงสร้าง แม้ในช่วงวิกฤตสำคัญ เช่น Global Financial Crisis, COVID-19 และ Inflation shock ซึ่งสะท้อนถึงความแข็งแกร่งและความทนทานของ Quality factor ในหลายสภาวะตลาด

![underwater_plot](assets/underwater_plot.png)
*Figure 22: Underwater plot เปรียบเทียบ drawdown ของพอร์ต Quality, Junk และ S&P 500 แสดงให้เห็นว่า Quality มี drawdown ที่ตื้นกว่า (MDD -52.4% vs -59.9%) และใช้เวลาฟื้นตัวสั้นกว่า Junk โดยมีระยะเวลาอยู่ใต้น้ำ (days underwater) ใกล้เคียงกับตลาด*

ผลลัพธ์สะท้อนว่า Quality portfolio ไม่เพียงให้ผลตอบแทนที่สูงกว่า แต่ยังมีลักษณะการขาดทุนที่ควบคุมได้ดีกว่าในช่วงวิกฤต โดยมี drawdown ที่ไม่ลึกเท่า Junk และระยะเวลาฟื้นตัวที่สั้นกว่าในหลายช่วงเวลา แม้ในเหตุการณ์รุนแรง เช่น Global Financial Crisis และ COVID-19 ขณะที่เมื่อเทียบกับตลาดโดยรวม Quality มีพฤติกรรมความเสี่ยงที่ใกล้เคียง แต่สามารถสร้างผลตอบแทนที่ดีกว่า ซึ่งบ่งชี้ถึงประสิทธิภาพของ factor ในเชิง risk-adjusted return

![annual_return](assets/annual_return.png)
*Figure 23: ผลตอบแทนรายปีของพอร์ต Quality, Junk และ S&P 500 พร้อมค่า excess return ของ Quality เทียบกับตลาด แสดงให้เห็นว่า Quality outperform ตลาดในหลายช่วงเวลา แม้จะมีบางปีที่ underperform*

ผลตอบแทนรายปีแสดงให้เห็นว่า Quality portfolio outperform ตลาดในหลายช่วงเวลา โดยเฉพาะในช่วงฟื้นตัวหลังวิกฤต แม้จะมีบางปีที่ underperform ซึ่งสะท้อน regime dependency ของ factor investing ที่แปรผันตามสภาวะตลาด โดยรวมยังยืนยันว่า Quality เป็นปัจจัยที่มีความสามารถในการสร้างผลตอบแทนระยะยาว แม้จะมีความผันผวนในระยะสั้น

### Risk Characteristics 
![rolling_beta](assets/rolling_beta.png)
*Figure 24: ค่า rolling beta และ correlation ของพอร์ต Quality และ Junk เทียบกับ S&P 500 แสดงให้เห็นว่า Quality มี beta ต่ำกว่า Junk อย่างสม่ำเสมอ และมี correlation กับตลาดในระดับสูงใกล้เคียงกัน*

ผลลัพธ์สะท้อนว่า Quality portfolio มีความอ่อนไหวต่อความผันผวนของตลาด (market sensitivity) ต่ำกว่า Junk โดยมีค่า beta ต่ำกว่าอย่างต่อเนื่องในหลายช่วงเวลา ขณะที่ correlation กับตลาดยังคงอยู่ในระดับสูง ซึ่งบ่งชี้ว่าผลตอบแทนของ Quality ไม่ได้เกิดจากการหลีกเลี่ยงตลาด (low exposure) แต่เกิดจากคุณภาพของหุ้นที่เลือก นอกจากนี้การที่ beta ของ Quality มีแนวโน้มลดลงในช่วงหลังยังสะท้อนถึงลักษณะ defensive ของพอร์ตในบางช่วงเศรษฐกิจ

## Conclusion
การวิเคราะห์ Quality Minus Junk (QMJ) แสดงให้เห็นว่า “คุณภาพของบริษัท” เป็นปัจจัยที่สามารถอธิบายความแตกต่างของผลตอบแทนได้อย่างมีนัยสำคัญ โดย Profitability เป็นตัวขับเคลื่อนหลักของ Quality ขณะที่ Safety ทำหน้าที่ลดความเสี่ยงและเสริมความทนทานของพอร์ต และ Growth ทำหน้าที่เป็นปัจจัยเสริมที่ช่วยยืนยันความต่อเนื่องของคุณภาพในระยะยาว

ผลการทดสอบเชิงประจักษ์ชี้ให้เห็นว่า Quality portfolio สามารถสร้างผลตอบแทนสะสมที่สูงกว่า พร้อมความเสี่ยงที่ต่ำกว่า ทั้งในแง่ของ drawdown และ market sensitivity โดย long-short strategy (Quality − Junk) ให้ spread เป็นบวกอย่างต่อเนื่อง สะท้อนถึงความแข็งแกร่งของ factor ในหลายสภาวะตลาด

แม้จะมีบางช่วงที่ underperform ซึ่งสะท้อนลักษณะ regime dependency ของ factor investing แต่โดยรวม Quality ยังคงเป็นปัจจัยที่มีความสม่ำเสมอ เสถียร และสามารถสร้างผลตอบแทนระยะยาวได้อย่างมีประสิทธิภาพ

โดยสรุป Quality factor ไม่ได้เป็นเพียงภาพสะท้อนของกำไรในปัจจุบัน แต่เป็นการรวมกันของความสามารถในการทำกำไร โครงสร้างทางการเงินที่แข็งแกร่ง และการเติบโตที่ยั่งยืน ซึ่งร่วมกันสร้างสัญญาณการลงทุนที่มีความน่าเชื่อถือในระยะยาว

## References

- Asness, C. S., Frazzini, A., & Pedersen, L. H. (2019). *Quality Minus Junk*. Review of Accounting Studies.