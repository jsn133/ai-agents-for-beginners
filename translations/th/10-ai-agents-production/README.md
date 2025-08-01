<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "8164484c16b1ed3287ef9dae9fc437c1",
  "translation_date": "2025-07-23T08:47:36+00:00",
  "source_file": "10-ai-agents-production/README.md",
  "language_code": "th"
}
-->
# เอเจนต์ AI ในการใช้งานจริง: การสังเกตและการประเมินผล

เมื่อเอเจนต์ AI เปลี่ยนจากต้นแบบทดลองไปสู่การใช้งานในโลกจริง ความสามารถในการเข้าใจพฤติกรรมของพวกเขา ตรวจสอบประสิทธิภาพ และประเมินผลลัพธ์อย่างเป็นระบบกลายเป็นสิ่งสำคัญ

## เป้าหมายการเรียนรู้

หลังจากจบบทเรียนนี้ คุณจะสามารถ:
- เข้าใจแนวคิดหลักเกี่ยวกับการสังเกตและการประเมินผลของเอเจนต์
- ใช้เทคนิคในการปรับปรุงประสิทธิภาพ ค่าใช้จ่าย และความมีประสิทธิผลของเอเจนต์
- รู้ว่าควรประเมินเอเจนต์ AI อย่างไรและประเมินอะไร
- ควบคุมค่าใช้จ่ายเมื่อใช้งานเอเจนต์ AI ในการผลิต
- ใช้เครื่องมือในการติดตามเอเจนต์ที่สร้างด้วย AutoGen

เป้าหมายคือการให้ความรู้ที่ช่วยเปลี่ยนเอเจนต์แบบ "กล่องดำ" ให้กลายเป็นระบบที่โปร่งใส จัดการได้ และเชื่อถือได้

_**หมายเหตุ:** การใช้งานเอเจนต์ AI ที่ปลอดภัยและน่าเชื่อถือเป็นสิ่งสำคัญ ดูบทเรียน [การสร้างเอเจนต์ AI ที่น่าเชื่อถือ](./06-building-trustworthy-agents/README.md) เพื่อข้อมูลเพิ่มเติม_

## การติดตามและการแบ่งขั้นตอน

เครื่องมือการสังเกต เช่น [Langfuse](https://langfuse.com/) หรือ [Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-azure-ai-foundry) มักแสดงการทำงานของเอเจนต์ในรูปแบบการติดตามและการแบ่งขั้นตอน

- **การติดตาม (Trace)** หมายถึงงานของเอเจนต์ที่สมบูรณ์ตั้งแต่ต้นจนจบ (เช่น การจัดการคำถามของผู้ใช้)
- **การแบ่งขั้นตอน (Spans)** คือขั้นตอนย่อยแต่ละขั้นตอนภายในการติดตาม (เช่น การเรียกใช้โมเดลภาษา หรือการดึงข้อมูล)

![Trace tree in Langfuse](https://langfuse.com/images/cookbook/example-autogen-evaluation/trace-tree.png)

หากไม่มีการสังเกต เอเจนต์ AI อาจดูเหมือน "กล่องดำ" ซึ่งสถานะภายในและเหตุผลของมันไม่ชัดเจน ทำให้ยากต่อการวิเคราะห์ปัญหาหรือปรับปรุงประสิทธิภาพ แต่ด้วยการสังเกต เอเจนต์จะกลายเป็น "กล่องแก้ว" ที่ให้ความโปร่งใส ซึ่งเป็นสิ่งสำคัญสำหรับการสร้างความไว้วางใจและการทำงานที่ถูกต้องตามที่ตั้งใจไว้

## ทำไมการสังเกตจึงสำคัญในสภาพแวดล้อมการผลิต

การเปลี่ยนเอเจนต์ AI ไปสู่สภาพแวดล้อมการผลิตนำมาซึ่งความท้าทายและข้อกำหนดใหม่ การสังเกตไม่ใช่แค่ "สิ่งที่ควรมี" แต่เป็นความสามารถที่สำคัญ:

*   **การแก้ไขปัญหาและการวิเคราะห์ต้นเหตุ**: เมื่อเอเจนต์ล้มเหลวหรือให้ผลลัพธ์ที่ไม่คาดคิด เครื่องมือการสังเกตจะให้ข้อมูลการติดตามที่จำเป็นในการระบุแหล่งที่มาของข้อผิดพลาด ซึ่งสำคัญอย่างยิ่งในเอเจนต์ที่ซับซ้อนซึ่งอาจเกี่ยวข้องกับการเรียกใช้ LLM หลายครั้ง การโต้ตอบกับเครื่องมือ และตรรกะตามเงื่อนไข
*   **การจัดการเวลาและค่าใช้จ่าย**: เอเจนต์ AI มักพึ่งพา LLM และ API ภายนอกที่มีค่าใช้จ่ายตามจำนวนโทเค็นหรือการเรียกใช้ การสังเกตช่วยติดตามการเรียกใช้เหล่านี้อย่างแม่นยำ ช่วยระบุการดำเนินการที่ช้าเกินไปหรือมีค่าใช้จ่ายสูงเกินไป ทำให้ทีมสามารถปรับปรุงคำสั่ง เลือกโมเดลที่มีประสิทธิภาพมากขึ้น หรือออกแบบเวิร์กโฟลว์ใหม่เพื่อจัดการค่าใช้จ่ายและให้ประสบการณ์ที่ดีแก่ผู้ใช้
*   **ความไว้วางใจ ความปลอดภัย และการปฏิบัติตามข้อกำหนด**: ในหลายแอปพลิเคชัน การรับรองว่าเอเจนต์ทำงานอย่างปลอดภัยและมีจริยธรรมเป็นสิ่งสำคัญ การสังเกตให้เส้นทางการตรวจสอบการกระทำและการตัดสินใจของเอเจนต์ ซึ่งสามารถใช้ในการตรวจจับและลดปัญหา เช่น การฉีดคำสั่ง การสร้างเนื้อหาที่เป็นอันตราย หรือการจัดการข้อมูลส่วนบุคคล (PII) อย่างไม่เหมาะสม ตัวอย่างเช่น คุณสามารถตรวจสอบการติดตามเพื่อเข้าใจว่าเหตุใดเอเจนต์จึงให้คำตอบหรือใช้เครื่องมือเฉพาะ
*   **วงจรการปรับปรุงอย่างต่อเนื่อง**: ข้อมูลการสังเกตเป็นพื้นฐานของกระบวนการพัฒนาที่มีการทำซ้ำ โดยการติดตามว่าเอเจนต์ทำงานอย่างไรในโลกจริง ทีมสามารถระบุพื้นที่ที่ต้องปรับปรุง รวบรวมข้อมูลเพื่อปรับแต่งโมเดล และตรวจสอบผลกระทบของการเปลี่ยนแปลง สิ่งนี้สร้างวงจรป้อนกลับที่ข้อมูลการผลิตจากการประเมินออนไลน์แจ้งการทดลองและการปรับปรุงแบบออฟไลน์ นำไปสู่ประสิทธิภาพของเอเจนต์ที่ดีขึ้นอย่างต่อเนื่อง

## เมตริกสำคัญที่ควรติดตาม

เพื่อการตรวจสอบและเข้าใจพฤติกรรมของเอเจนต์ ควรติดตามเมตริกและสัญญาณหลากหลายประเภท แม้ว่าเมตริกเฉพาะอาจแตกต่างกันไปตามวัตถุประสงค์ของเอเจนต์ แต่บางเมตริกมีความสำคัญในทุกกรณี

นี่คือเมตริกทั่วไปที่เครื่องมือการสังเกตมักติดตาม:

**เวลาในการตอบสนอง (Latency):** เอเจนต์ตอบสนองเร็วแค่ไหน? เวลารอที่นานเกินไปส่งผลเสียต่อประสบการณ์ของผู้ใช้ คุณควรวัดเวลาในการตอบสนองสำหรับงานและขั้นตอนย่อยโดยการติดตามการทำงานของเอเจนต์ ตัวอย่างเช่น เอเจนต์ที่ใช้เวลา 20 วินาทีในการเรียกใช้โมเดลทั้งหมดสามารถเร่งความเร็วได้โดยใช้โมเดลที่เร็วขึ้นหรือเรียกใช้โมเดลแบบขนาน

**ค่าใช้จ่าย (Costs):** ค่าใช้จ่ายต่อการทำงานของเอเจนต์คือเท่าไหร่? เอเจนต์ AI พึ่งพาการเรียกใช้ LLM ที่มีค่าใช้จ่ายตามจำนวนโทเค็นหรือ API ภายนอก การใช้เครื่องมือบ่อยครั้งหรือคำสั่งหลายครั้งสามารถเพิ่มค่าใช้จ่ายได้อย่างรวดเร็ว ตัวอย่างเช่น หากเอเจนต์เรียกใช้ LLM ห้าครั้งเพื่อปรับปรุงคุณภาพเพียงเล็กน้อย คุณต้องประเมินว่าค่าใช้จ่ายนั้นคุ้มค่าหรือไม่ หรือคุณสามารถลดจำนวนการเรียกใช้หรือใช้โมเดลที่ถูกกว่า การติดตามแบบเรียลไทม์ยังช่วยระบุการเพิ่มขึ้นที่ไม่คาดคิด (เช่น ข้อผิดพลาดที่ทำให้เกิดการวนลูป API มากเกินไป)

**ข้อผิดพลาดในการร้องขอ (Request Errors):** เอเจนต์ล้มเหลวในการร้องขอจำนวนเท่าไหร่? ซึ่งรวมถึงข้อผิดพลาดของ API หรือการเรียกใช้เครื่องมือที่ล้มเหลว เพื่อทำให้เอเจนต์ของคุณมีความทนทานมากขึ้นในสภาพแวดล้อมการผลิต คุณสามารถตั้งค่าการสำรองข้อมูลหรือการลองใหม่ได้ เช่น หากผู้ให้บริการ LLM A ไม่สามารถใช้งานได้ คุณสามารถเปลี่ยนไปใช้ผู้ให้บริการ LLM B เป็นสำรอง

**ความคิดเห็นจากผู้ใช้ (User Feedback):** การประเมินโดยตรงจากผู้ใช้ให้ข้อมูลเชิงลึกที่มีค่า ซึ่งรวมถึงการให้คะแนนโดยตรง (👍/👎, ⭐1-5 ดาว) หรือความคิดเห็นเป็นข้อความ หากมีความคิดเห็นเชิงลบอย่างต่อเนื่อง ควรแจ้งเตือนคุณเนื่องจากเป็นสัญญาณว่าเอเจนต์ทำงานไม่เป็นไปตามที่คาดหวัง

**ความคิดเห็นโดยอ้อมจากผู้ใช้ (Implicit User Feedback):** พฤติกรรมของผู้ใช้ให้ความคิดเห็นโดยอ้อมแม้ไม่มีการให้คะแนนโดยตรง ซึ่งรวมถึงการแก้ไขคำถามทันที การถามคำถามซ้ำ หรือการคลิกปุ่มลองใหม่ ตัวอย่างเช่น หากคุณเห็นว่าผู้ใช้ถามคำถามเดิมซ้ำๆ นี่เป็นสัญญาณว่าเอเจนต์ทำงานไม่เป็นไปตามที่คาดหวัง

**ความถูกต้อง (Accuracy):** เอเจนต์ให้ผลลัพธ์ที่ถูกต้องหรือเป็นที่ต้องการบ่อยแค่ไหน? คำจำกัดความของความถูกต้องอาจแตกต่างกัน (เช่น ความถูกต้องในการแก้ปัญหา ความถูกต้องในการดึงข้อมูล ความพึงพอใจของผู้ใช้) ขั้นแรกคือการกำหนดว่า "ความสำเร็จ" สำหรับเอเจนต์ของคุณคืออะไร คุณสามารถติดตามความถูกต้องผ่านการตรวจสอบอัตโนมัติ คะแนนการประเมิน หรือป้ายกำกับการเสร็จสิ้นงาน ตัวอย่างเช่น การทำเครื่องหมายการติดตามว่า "สำเร็จ" หรือ "ล้มเหลว"

**เมตริกการประเมินอัตโนมัติ (Automated Evaluation Metrics):** คุณสามารถตั้งค่าการประเมินอัตโนมัติได้ ตัวอย่างเช่น คุณสามารถใช้ LLM เพื่อให้คะแนนผลลัพธ์ของเอเจนต์ เช่น ว่ามีประโยชน์ ถูกต้อง หรือไม่ นอกจากนี้ยังมีไลบรารีโอเพ่นซอร์สหลายตัวที่ช่วยให้คุณให้คะแนนแง่มุมต่างๆ ของเอเจนต์ เช่น [RAGAS](https://docs.ragas.io/) สำหรับเอเจนต์ RAG หรือ [LLM Guard](https://llm-guard.com/) เพื่อตรวจจับภาษาที่เป็นอันตรายหรือการฉีดคำสั่ง

ในทางปฏิบัติ การรวมเมตริกเหล่านี้ให้ครอบคลุมสุขภาพของเอเจนต์ AI ได้ดีที่สุด ใน [สมุดบันทึกตัวอย่าง](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) ของบทนี้ เราจะแสดงให้เห็นว่าเมตริกเหล่านี้มีลักษณะอย่างไรในตัวอย่างจริง แต่ก่อนอื่นเราจะเรียนรู้ว่ากระบวนการประเมินทั่วไปมีลักษณะอย่างไร

## การติดตั้งเครื่องมือในเอเจนต์ของคุณ

เพื่อรวบรวมข้อมูลการติดตาม คุณจะต้องติดตั้งเครื่องมือในโค้ดของคุณ เป้าหมายคือการติดตั้งโค้ดเอเจนต์เพื่อส่งข้อมูลการติดตามและเมตริกที่สามารถจับ ประมวลผล และแสดงผลโดยแพลตฟอร์มการสังเกต

**OpenTelemetry (OTel):** [OpenTelemetry](https://opentelemetry.io/) ได้กลายเป็นมาตรฐานอุตสาหกรรมสำหรับการสังเกต LLM โดยให้ชุด API, SDK และเครื่องมือสำหรับการสร้าง รวบรวม และส่งออกข้อมูลการตรวจสอบ

มีไลบรารีการติดตั้งเครื่องมือหลายตัวที่ครอบคลุมเฟรมเวิร์กเอเจนต์ที่มีอยู่และทำให้ง่ายต่อการส่งออกการแบ่งขั้นตอน OpenTelemetry ไปยังเครื่องมือการสังเกต ด้านล่างคือตัวอย่างการติดตั้งเอเจนต์ AutoGen ด้วยไลบรารีการติดตั้งเครื่องมือ [OpenLit](https://github.com/openlit/openlit):

```python
import openlit

openlit.init(tracer = langfuse._otel_tracer, disable_batch = True)
```

[สมุดบันทึกตัวอย่าง](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) ในบทนี้จะแสดงวิธีการติดตั้งเครื่องมือในเอเจนต์ AutoGen ของคุณ

**การสร้างการแบ่งขั้นตอนด้วยตนเอง:** แม้ว่าไลบรารีการติดตั้งเครื่องมือจะให้พื้นฐานที่ดี แต่บ่อยครั้งที่ต้องการข้อมูลที่ละเอียดหรือกำหนดเองมากขึ้น คุณสามารถสร้างการแบ่งขั้นตอนด้วยตนเองเพื่อเพิ่มตรรกะของแอปพลิเคชันที่กำหนดเองได้ สิ่งสำคัญคือสามารถเพิ่มข้อมูลที่กำหนดเองลงในการแบ่งขั้นตอนที่สร้างขึ้นโดยอัตโนมัติหรือด้วยตนเองได้ เช่น `user_id`, `session_id` หรือ `model_version`

ตัวอย่างการสร้างการติดตามและการแบ่งขั้นตอนด้วยตนเองด้วย [Langfuse Python SDK](https://langfuse.com/docs/sdk/python/sdk-v3):

```python
from langfuse import get_client
 
langfuse = get_client()
 
span = langfuse.start_span(name="my-span")
 
span.end()
```

## การประเมินเอเจนต์

การสังเกตให้เมตริก แต่การประเมินคือกระบวนการวิเคราะห์ข้อมูลนั้น (และการทดสอบ) เพื่อกำหนดว่าเอเจนต์ AI ทำงานได้ดีเพียงใดและสามารถปรับปรุงได้อย่างไร กล่าวอีกนัยหนึ่ง เมื่อคุณมีการติดตามและเมตริกเหล่านั้น คุณจะใช้มันในการตัดสินเอเจนต์และตัดสินใจอย่างไร

การประเมินผลเป็นประจำเป็นสิ่งสำคัญเนื่องจากเอเจนต์ AI มักไม่แน่นอนและสามารถเปลี่ยนแปลงได้ (ผ่านการอัปเดตหรือพฤติกรรมโมเดลที่เปลี่ยนแปลง) – หากไม่มีการประเมิน คุณจะไม่รู้ว่า "เอเจนต์อัจฉริยะ" ของคุณทำงานได้ดีจริงหรือไม่ หรือถ้ามันถดถอย

มีการประเมินผลสองประเภทสำหรับเอเจนต์ AI: **การประเมินออนไลน์** และ **การประเมินออฟไลน์** ทั้งสองมีคุณค่าและเสริมกัน เรามักเริ่มต้นด้วยการประเมินออฟไลน์ เนื่องจากนี่เป็นขั้นตอนขั้นต่ำที่จำเป็นก่อนการใช้งานเอเจนต์ใดๆ

### การประเมินออฟไลน์

![Dataset items in Langfuse](https://langfuse.com/images/cookbook/example-autogen-evaluation/example-dataset.png)

การประเมินนี้เกี่ยวข้องกับการประเมินเอเจนต์ในสภาพแวดล้อมที่ควบคุมได้ โดยทั่วไปใช้ชุดข้อมูลทดสอบ ไม่ใช่คำถามจากผู้ใช้จริง คุณใช้ชุดข้อมูลที่คัดสรรมา ซึ่งคุณรู้ว่าผลลัพธ์ที่คาดหวังหรือพฤติกรรมที่ถูกต้องคืออะไร จากนั้นจึงทดสอบเอเจนต์ของคุณกับชุดข้อมูลเหล่านั้น

ตัวอย่างเช่น หากคุณสร้างเอเจนต์แก้ปัญหาคำศัพท์คณิตศาสตร์ คุณอาจมี [ชุดข้อมูลทดสอบ](https://huggingface.co/datasets/gsm8k) จำนวน 100 ปัญหาพร้อมคำตอบที่ทราบ การประเมินออฟไลน์มักทำในระหว่างการพัฒนา (และสามารถเป็นส่วนหนึ่งของกระบวนการ CI/CD) เพื่อตรวจสอบการปรับปรุงหรือป้องกันการถดถอย ข้อดีคือมัน **สามารถทำซ้ำได้และคุณสามารถรับเมตริกความถูกต้องที่ชัดเจนเนื่องจากคุณมีข้อมูลจริง** คุณอาจจำลองคำถามของผู้ใช้และวัดการตอบสนองของเอเจนต์เทียบกับคำตอบที่เหมาะสม หรือใช้เมตริกอัตโนมัติที่กล่าวถึงข้างต้น

ความท้าทายหลักของการประเมินออฟไลน์คือการรับรองว่าชุดข้อมูลทดสอบของคุณครอบคลุมและยังคงเกี่ยวข้อง – เอเจนต์อาจทำงานได้ดีในชุดทดสอบที่กำหนด แต่พบคำถามที่แตกต่างกันมากในสภาพแวดล้อมการผลิต ดังนั้นคุณควรอัปเดตชุดทดสอบด้วยกรณีขอบใหม่และตัวอย่างที่สะท้อนสถานการณ์ในโลกจริง การผสมผสานระหว่างกรณี "การทดสอบเบื้องต้น" ขนาดเล็กและชุดการประเมินขนาดใหญ่มีประโยชน์: ชุดเล็กสำหรับการตรวจสอบอย่างรวดเร็วและชุดใหญ่สำหรับเมตริกประสิทธิภาพที่กว้างขึ้น

### การประเมินออนไลน์

![Observability metrics overview](https://langfuse.com/images/cookbook/example-autogen-evaluation/dashboard.png)

การประเมินนี้หมายถึงการประเมินเอเจนต์ในสภาพแวดล้อมจริงในโลกจริง กล่าวคือ ระหว่างการใช้งานจริงในสภาพแวดล้อมการผลิต การประเมินออนไลน์เกี่ยวข้องกับการติดตามประสิทธิภาพของเอเจนต์ในปฏิสัมพันธ์ของผู้ใช้จริงและการวิเคราะห์ผลลัพธ์อย่างต่อเนื่อง

ตัวอย่างเช่น คุณอาจติดตามอัตราความสำเร็จ คะแนนความพึงพอใจของผู้ใช้ หรือเมตริกอื่นๆ ในการใช้งานจริง ข้อดีของการประเมินออนไลน์คือมัน **จับสิ่งที่คุณอาจไม่คาดคิดในสภาพแวดล้อมทดลอง** – คุณสามารถสังเกตการเปลี่ยนแปลงของโมเดลเมื่อเวลาผ่านไป (หากประสิทธิภาพของเอเจนต์ลดลงเมื่อรูปแบบการป้อนข้อมูลเปลี่ยนแปลง) และจับคำถามหรือสถานการณ์ที่ไม่คาดคิดซึ่งไม่ได้อยู่ในชุดข้อมูลทดสอบ มันให้ภาพที่แท้จริงว่าเอเจนต์ทำงานอย่างไรในโลกจริง

การประเมินออนไลน์มักเกี่ยวข้องกับการรวบรวมความคิดเห็นโดยอ้อมและโดยตรงจากผู้ใช้ตามที่กล่าวถึง และอาจรวมถึงการทดสอบเงาหรือการทดสอบ A/B (ที่เวอร์ชันใหม่ของเอเจนต์ทำ

- ปรับแต่งพารามิเตอร์, คำสั่ง, และการตั้งชื่อเครื่องมือให้เหมาะสม  |
| ระบบ Multi-Agent ทำงานไม่สม่ำเสมอ | - ปรับแต่งคำสั่งที่ให้กับแต่ละเอเจนต์เพื่อให้มีความเฉพาะเจาะจงและแตกต่างกันอย่างชัดเจน<br>- สร้างระบบลำดับชั้นโดยใช้ "routing" หรือเอเจนต์ควบคุมเพื่อกำหนดว่าเอเจนต์ใดเหมาะสมที่สุด |

ปัญหาหลายอย่างเหล่านี้สามารถระบุได้อย่างมีประสิทธิภาพมากขึ้นเมื่อมีระบบการสังเกตการณ์ (observability) ข้อมูลการติดตาม (traces) และเมตริกที่เราได้พูดถึงก่อนหน้านี้ช่วยชี้จุดที่เกิดปัญหาในกระบวนการทำงานของเอเจนต์ได้อย่างแม่นยำ ทำให้การแก้ไขข้อบกพร่องและการปรับปรุงประสิทธิภาพมีประสิทธิผลมากขึ้น

## การจัดการต้นทุน

นี่คือกลยุทธ์บางประการในการจัดการต้นทุนสำหรับการนำ AI agents ไปใช้งานในระบบจริง:

**การใช้โมเดลขนาดเล็ก:** Small Language Models (SLMs) สามารถทำงานได้ดีในบางกรณีการใช้งานของเอเจนต์ และช่วยลดต้นทุนได้อย่างมาก ดังที่ได้กล่าวไว้ก่อนหน้านี้ การสร้างระบบประเมินผลเพื่อเปรียบเทียบประสิทธิภาพระหว่างโมเดลขนาดเล็กและขนาดใหญ่เป็นวิธีที่ดีที่สุดในการทำความเข้าใจว่า SLM จะทำงานได้ดีเพียงใดในกรณีการใช้งานของคุณ พิจารณาใช้ SLM สำหรับงานที่ง่าย เช่น การจำแนกเจตนา (intent classification) หรือการดึงพารามิเตอร์ (parameter extraction) และสำรองโมเดลขนาดใหญ่ไว้สำหรับงานที่ต้องใช้การวิเคราะห์ที่ซับซ้อน

**การใช้ Router Model:** กลยุทธ์ที่คล้ายกันคือการใช้โมเดลที่หลากหลายทั้งในด้านขนาดและประเภท คุณสามารถใช้ LLM/SLM หรือฟังก์ชันแบบ serverless เพื่อกำหนดเส้นทางคำขอไปยังโมเดลที่เหมาะสมที่สุดตามความซับซ้อน วิธีนี้จะช่วยลดต้นทุนในขณะที่ยังคงรักษาประสิทธิภาพในงานที่เหมาะสม ตัวอย่างเช่น ส่งคำถามง่าย ๆ ไปยังโมเดลขนาดเล็กที่เร็วกว่า และใช้โมเดลขนาดใหญ่ที่มีต้นทุนสูงสำหรับงานที่ต้องการการวิเคราะห์ที่ซับซ้อน

**การแคชคำตอบ:** การระบุคำขอและงานที่พบบ่อย และจัดเตรียมคำตอบไว้ล่วงหน้าก่อนที่จะผ่านระบบเอเจนต์ของคุณ เป็นวิธีที่ดีในการลดปริมาณคำขอที่คล้ายกัน คุณยังสามารถสร้างกระบวนการเพื่อระบุว่าคำขอใหม่มีความคล้ายคลึงกับคำขอที่แคชไว้มากน้อยเพียงใดโดยใช้โมเดล AI ที่ง่ายกว่า กลยุทธ์นี้สามารถลดต้นทุนได้อย่างมากสำหรับคำถามที่พบบ่อยหรือกระบวนการทำงานที่ซ้ำ ๆ

## มาดูตัวอย่างการใช้งานจริง

ใน [notebook ตัวอย่างของส่วนนี้](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) เราจะเห็นตัวอย่างของการใช้เครื่องมือสังเกตการณ์เพื่อติดตามและประเมินผลเอเจนต์ของเรา

## บทเรียนก่อนหน้า

[Metacognition Design Pattern](../09-metacognition/README.md)

## บทเรียนถัดไป

[MCP](../11-mcp/README.md)

**ข้อจำกัดความรับผิดชอบ**:  
เอกสารนี้ได้รับการแปลโดยใช้บริการแปลภาษา AI [Co-op Translator](https://github.com/Azure/co-op-translator) แม้ว่าเราจะพยายามให้การแปลมีความถูกต้อง แต่โปรดทราบว่าการแปลโดยอัตโนมัติอาจมีข้อผิดพลาดหรือความไม่ถูกต้อง เอกสารต้นฉบับในภาษาต้นทางควรถือเป็นแหล่งข้อมูลที่เชื่อถือได้ สำหรับข้อมูลที่สำคัญ ขอแนะนำให้ใช้บริการแปลภาษามนุษย์ที่เป็นมืออาชีพ เราไม่รับผิดชอบต่อความเข้าใจผิดหรือการตีความที่ผิดพลาดซึ่งเกิดจากการใช้การแปลนี้