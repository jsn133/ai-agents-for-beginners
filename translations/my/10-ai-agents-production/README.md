<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "8164484c16b1ed3287ef9dae9fc437c1",
  "translation_date": "2025-07-23T09:21:11+00:00",
  "source_file": "10-ai-agents-production/README.md",
  "language_code": "my"
}
-->
# AI Agents in Production: Observability & Evaluation

AI အေးဂျင့်များကို စမ်းသပ်မှုအဆင့်မှ အမှန်တကယ်အသုံးချနိုင်သော အက်ပလီကေးရှင်းများသို့ ရွှေ့ပြောင်းသည့်အခါ၊ ၎င်းတို့၏ အပြုအမူကို နားလည်နိုင်ခြင်း၊ စွမ်းဆောင်ရည်ကို စောင့်ကြည့်နိုင်ခြင်းနှင့် အထွေထွေထွက်လဒ်များကို စနစ်တကျ အကဲဖြတ်နိုင်ခြင်းသည် အရေးပါလာသည်။

## သင်ယူရမည့်အရာများ

ဒီသင်ခန်းစာပြီးဆုံးပြီးနောက်၊ သင်သည် အောက်ပါအရာများကို သိရှိနားလည်နိုင်ပါမည်။
- အေးဂျင့်၏ စောင့်ကြည့်နိုင်မှုနှင့် အကဲဖြတ်မှုဆိုင်ရာ အဓိကအယူအဆများ
- အေးဂျင့်များ၏ စွမ်းဆောင်ရည်၊ ကုန်ကျစရိတ်နှင့် ထိရောက်မှုကို တိုးတက်စေရန် နည်းလမ်းများ
- သင့် AI အေးဂျင့်များကို စနစ်တကျ အကဲဖြတ်ရမည့်အရာများနှင့် နည်းလမ်းများ
- AI အေးဂျင့်များကို ထုတ်လုပ်မှုအဆင့်တွင် အသုံးချသည့်အခါ ကုန်ကျစရိတ်ကို ထိန်းချုပ်နည်း
- AutoGen ဖြင့် တည်ဆောက်ထားသော အေးဂျင့်များကို စောင့်ကြည့်ရန် နည်းလမ်းများ

ဤသင်ခန်းစာ၏ ရည်ရွယ်ချက်မှာ "အနက်ရောင်သေတ္တာ" အေးဂျင့်များကို ပွင့်လင်းမြင်သာသော၊ စီမံခန့်ခွဲနိုင်သော၊ ယုံကြည်စိတ်ချရသော စနစ်များအဖြစ် ပြောင်းလဲနိုင်ရန် သင့်အား အသိပညာပေးခြင်းဖြစ်သည်။

_**မှတ်ချက်:** ယုံကြည်စိတ်ချရသော AI အေးဂျင့်များကို ထုတ်လုပ်မှုအဆင့်တွင် အသုံးချရန် အရေးကြီးပါသည်။ [Building Trustworthy AI Agents](./06-building-trustworthy-agents/README.md) သင်ခန်းစာကို ကြည့်ရှုပါ။_

## Traces and Spans

[Langfuse](https://langfuse.com/) သို့မဟုတ် [Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-azure-ai-foundry) ကဲ့သို့သော စောင့်ကြည့်မှုကိရိယာများသည် အေးဂျင့်၏ လုပ်ဆောင်မှုများကို trace နှင့် span အဖြစ် ဖော်ပြလေ့ရှိသည်။

- **Trace** သည် အေးဂျင့်၏ တစ်ခုလုံးသော တာဝန်ကို အစမှ အဆုံးအထိ ကိုယ်စားပြုသည် (ဥပမာ - အသုံးပြုသူမေးခွန်းကို ကိုင်တွယ်ခြင်း)။
- **Spans** သည် trace အတွင်းရှိ တစ်ခုချင်းစီသော အဆင့်များကို ကိုယ်စားပြုသည် (ဥပမာ - ဘာသာစကားမော်ဒယ်ကို ခေါ်ယူခြင်း သို့မဟုတ် ဒေတာကို ရယူခြင်း)။

![Trace tree in Langfuse](https://langfuse.com/images/cookbook/example-autogen-evaluation/trace-tree.png)

စောင့်ကြည့်မှုမရှိပါက၊ AI အေးဂျင့်သည် "အနက်ရောင်သေတ္တာ" ကဲ့သို့ ခံစားရနိုင်သည် - ၎င်း၏ အတွင်းအခြေအနေနှင့် အကြောင်းပြချက်များသည် မမြင်သာသောကြောင့် ပြဿနာများကို ရှာဖွေခြင်း သို့မဟုတ် စွမ်းဆောင်ရည်ကို အကောင်းဆုံးဖြစ်စေရန် ခက်ခဲနိုင်သည်။ စောင့်ကြည့်မှုရှိပါက၊ အေးဂျင့်များသည် "ဖန်သားသေတ္တာ" ကဲ့သို့ ဖြစ်လာပြီး ယုံကြည်မှုတည်ဆောက်ရန်နှင့် ၎င်းတို့ရည်ရွယ်သည့်အတိုင်း လုပ်ဆောင်နေကြောင်း သေချာစေရန် အရေးပါသော ပွင့်လင်းမြင်သာမှုကို ပေးစွမ်းသည်။

## ထုတ်လုပ်မှုပတ်ဝန်းကျင်တွင် စောင့်ကြည့်မှု၏ အရေးပါမှု

AI အေးဂျင့်များကို ထုတ်လုပ်မှုပတ်ဝန်းကျင်သို့ ရွှေ့ပြောင်းခြင်းသည် စိန်ခေါ်မှုအသစ်များနှင့် လိုအပ်ချက်များကို ဖန်တီးပေးသည်။ စောင့်ကြည့်မှုသည် "လိုအပ်လို့ရှိ" သက်သက်မဟုတ်တော့ဘဲ အရေးပါသော စွမ်းရည်တစ်ခုဖြစ်လာသည်။

*   **ပြဿနာရှာဖွေခြင်းနှင့် အကြောင်းရင်းခွဲခြမ်းစိတ်ဖြာခြင်း**: အေးဂျင့်သည် မအောင်မြင်ခြင်း သို့မဟုတ် မမျှော်လင့်ထားသော ထွက်လဒ်ကို ဖန်တီးသောအခါ၊ စောင့်ကြည့်မှုကိရိယာများသည် ပြဿနာရင်းမြစ်ကို ရှာဖွေရန် လိုအပ်သော trace များကို ပေးစွမ်းသည်။ ၎င်းသည် LLM ခေါ်ယူမှုများ၊ ကိရိယာအပြန်အလှန်များနှင့် အခြေအနေအခြေခံသဘောတရားများပါဝင်နိုင်သော ရှုပ်ထွေးသော အေးဂျင့်များတွင် အထူးအရေးပါသည်။
*   **နောက်ကျမှုနှင့် ကုန်ကျစရိတ် စီမံခန့်ခွဲမှု**: AI အေးဂျင့်များသည် token သို့မဟုတ် call တစ်ခုချင်းစီအတွက် ကုန်ကျစရိတ်ဖြင့် တိုင်းတာသော LLM များနှင့် အခြား API များကို အားထားလေ့ရှိသည်။ စောင့်ကြည့်မှုသည် ၎င်းတို့၏ call များကို တိကျစွာ ခြေရာခံရန် ခွင့်ပြုသည်။ ၎င်းသည် အလွန်နှေးကွေး သို့မဟုတ် ကုန်ကျစရိတ်များသော လုပ်ဆောင်မှုများကို ဖော်ထုတ်ရန် ကူညီပြီး prompt များကို အကောင်းဆုံးဖြစ်စေရန်၊ ပိုထိရောက်သော မော်ဒယ်များကို ရွေးချယ်ရန် သို့မဟုတ် workflow များကို ပြန်လည်ဒီဇိုင်းဆွဲရန် ခွင့်ပြုသည်။
*   **ယုံကြည်မှု၊ လုံခြုံမှုနှင့် အညီအနေ**: အက်ပလီကေးရှင်းများစွာတွင် အေးဂျင့်များသည် လုံခြုံစွာနှင့် ကျင့်သုံးမှုအရ လုပ်ဆောင်ကြောင်း သေချာစေရန် အရေးကြီးသည်။ စောင့်ကြည့်မှုသည် အေးဂျင့်၏ လုပ်ဆောင်မှုများနှင့် ဆုံးဖြတ်ချက်များ၏ စစ်ဆေးမှုလမ်းကြောင်းကို ပေးစွမ်းသည်။ ၎င်းကို prompt injection၊ အန္တရာယ်ရှိသော အကြောင်းအရာ ဖန်တီးခြင်း သို့မဟုတ် ကိုယ်ရေးအချက်အလက်များကို မသင့်တော်စွာ ကိုင်တွယ်ခြင်းကဲ့သို့သော ပြဿနာများကို ရှာဖွေပြီး ဖြေရှင်းရန် အသုံးပြုနိုင်သည်။
*   **ဆက်လက်တိုးတက်မှုလှိုင်းများ**: စောင့်ကြည့်မှုဒေတာသည် iterative development လုပ်ငန်းစဉ်၏ အခြေခံဖြစ်သည်။ အေးဂျင့်များသည် အမှန်တကယ်ကမ္ဘာတွင် လုပ်ဆောင်ပုံကို စောင့်ကြည့်ခြင်းအားဖြင့် တိုးတက်မှုအပိုင်းများကို ဖော်ထုတ်ရန်၊ မော်ဒယ်များကို fine-tuning ပြုလုပ်ရန် ဒေတာကို စုဆောင်းရန်နှင့် ပြောင်းလဲမှုများ၏ သက်ရောက်မှုကို အတည်ပြုရန် ခွင့်ပြုသည်။

## စောင့်ကြည့်ရန် အဓိက Metrics

အေးဂျင့်၏ အပြုအမူကို စောင့်ကြည့်ရန်နှင့် နားလည်ရန်၊ metrics များနှင့် signal များစွာကို ခြေရာခံရမည်။ Metrics များသည် အေးဂျင့်၏ ရည်ရွယ်ချက်အပေါ် မူတည်၍ ကွဲပြားနိုင်သော်လည်း အချို့သည် အားလုံးအတွက် အရေးပါသည်။

အေးဂျင့်၏ ကျန်းမာရေးကို စောင့်ကြည့်ရန် အရေးပါသော metrics များမှာ အောက်ပါအတိုင်းဖြစ်သည်။

**Latency:** အေးဂျင့်သည် အမြန်ဆုံး တုံ့ပြန်နိုင်ပါသလား။ အချိန်ကြာမြင့်ခြင်းသည် အသုံးပြုသူအတွေ့အကြုံကို အနုတ်လက္ခဏာသက်ရောက်စေသည်။ Latency ကို တာဝန်များနှင့် တစ်ခုချင်းစီသော အဆင့်များအတွက် trace ခြေရာခံခြင်းအားဖြင့် တိုင်းတာရမည်။

**Costs:** အေးဂျင့်တစ်ခုလုံး၏ ကုန်ကျစရိတ်သည် ဘယ်လောက်ရှိပါသလဲ။ AI အေးဂျင့်များသည် token သို့မဟုတ် external API များကို အားထားလေ့ရှိသည်။ Tool များကို မကြာခဏ အသုံးပြုခြင်း သို့မဟုတ် prompt များစွာကို အသုံးပြုခြင်းသည် ကုန်ကျစရိတ်ကို မြန်မြန်တက်စေသည်။

**Request Errors:** အေးဂျင့်သည် ဘယ်လောက် request မအောင်မြင်ခဲ့ပါသလဲ။ API error များ သို့မဟုတ် tool call မအောင်မြင်မှုများ ပါဝင်နိုင်သည်။

**User Feedback:** အသုံးပြုသူများ၏ တိုက်ရိုက်အကဲဖြတ်မှုများသည် တန်ဖိုးရှိသော အမြင်များကို ပေးစွမ်းသည်။

**Implicit User Feedback:** အသုံးပြုသူအပြုအမူများသည် တိုက်ရိုက်မဟုတ်သော အကဲဖြတ်မှုများကို ပေးစွမ်းသည်။

**Accuracy:** အေးဂျင့်သည် မှန်ကန်သော သို့မဟုတ် ရလဒ်ကောင်းများကို ဘယ်လောက်ကြိမ် ဖန်တီးနိုင်ပါသလဲ။

**Automated Evaluation Metrics:** Automated evals များကို သတ်မှတ်နိုင်သည်။

အေးဂျင့်၏ ကျန်းမာရေးကို အကောင်းဆုံးဖော်ထုတ်ရန်၊ metrics များ၏ ပေါင်းစပ်မှုသည် အကောင်းဆုံး coverage ကို ပေးစွမ်းသည်။

## သင့်အေးဂျင့်ကို Instrument လုပ်ခြင်း

Tracing ဒေတာကို စုဆောင်းရန်၊ သင့် code ကို instrument လုပ်ရန် လိုအပ်သည်။

**OpenTelemetry (OTel):** [OpenTelemetry](https://opentelemetry.io/) သည် LLM စောင့်ကြည့်မှုအတွက် စက်မှုလုပ်ငန်းစံဖြစ်လာသည်။

```python
import openlit

openlit.init(tracer = langfuse._otel_tracer, disable_batch = True)
```

ဤအခန်း၏ [example notebook](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) တွင် သင့် AutoGen အေးဂျင့်ကို instrument လုပ်နည်းကို ပြသမည်။

**Manual Span Creation:** Instrumentation libraries များသည် အခြေခံကို ပေးစွမ်းသော်လည်း၊ အချို့သော အခြေအနေများတွင် ပိုမိုအသေးစိတ် သို့မဟုတ် စိတ်ကြိုက်အချက်အလက်များ လိုအပ်နိုင်သည်။

```python
from langfuse import get_client
 
langfuse = get_client()
 
span = langfuse.start_span(name="my-span")
 
span.end()
```

## အေးဂျင့်အကဲဖြတ်ခြင်း

စောင့်ကြည့်မှုသည် metrics များကို ပေးစွမ်းသော်လည်း၊ အကဲဖြတ်ခြင်းသည် ၎င်းဒေတာကို ခွဲခြမ်းစိတ်ဖြာခြင်း (နှင့် စမ်းသပ်မှုများ ပြုလုပ်ခြင်း) ဖြစ်ပြီး AI အေးဂျင့်၏ စွမ်းဆောင်ရည်ကောင်းမွန်မှုကို သတ်မှတ်ရန်နှင့် တိုးတက်စေရန် ဖြစ်သည်။

AI အေးဂျင့်များသည် non-deterministic ဖြစ်ပြီး update များ သို့မဟုတ် model behavior များ ပြောင်းလဲမှုကြောင့် တိုးတက်နိုင်သည့်အတွက်၊ အကဲဖြတ်မှုမရှိပါက အေးဂျင့်သည် ၎င်း၏ တာဝန်ကို ကောင်းစွာ လုပ်ဆောင်နေကြောင်း သေချာမရနိုင်ပါ။

AI အေးဂျင့်များအတွက် အကဲဖြတ်မှုများကို **online evaluation** နှင့် **offline evaluation** ဟူ၍ အမျိုးအစားနှစ်မျိုးခွဲခြားနိုင်သည်။

### Offline Evaluation

![Dataset items in Langfuse](https://langfuse.com/images/cookbook/example-autogen-evaluation/example-dataset.png)

Offline evaluation သည် စမ်းသပ်မှု dataset များကို အသုံးပြု၍ အေးဂျင့်ကို စမ်းသပ်ခြင်းဖြစ်သည်။

### Online Evaluation 

![Observability metrics overview](https://langfuse.com/images/cookbook/example-autogen-evaluation/dashboard.png)

Online evaluation သည် အေးဂျင့်ကို အမှန်တကယ် အသုံးပြုမှုအခြေအနေတွင် စမ်းသပ်ခြင်းဖြစ်သည်။

### နှစ်မျိုးပေါင်းစပ်ခြင်း

Online နှင့် offline အကဲဖြတ်မှုများသည် အချင်းချင်း အပြန်အလှန်ဖြည့်ဆည်းပေးနိုင်သည်။

## ရှိနိုင်သော ပြဿနာများ

AI အေးဂျင့်များကို ထုတ်လုပ်မှုအဆင့်သို့ ရွှေ့ပြောင်းသည့်အခါ၊ အမျိုးမျိုးသော စိန်ခေါ်မှုများနှင့် ရင်ဆိုင်ရနိုင်သည်။ 

| **ပြဿနာ**    | **ဖြေရှင်းနည်း**   |
| ------------- | ------------------ |
| AI အေးဂျင့်သည် တာဝန်များကို တိကျစွာ မလုပ်ဆောင်နိုင်ခြင်း | - AI အေးဂျင့်ကို ပေးသော prompt ကို ပြန်လည်တိုးတက်အောင် ပြုလုပ်ပါ။<br>- တာဝန်များကို subtask များအဖြစ် ခွဲခြားပြီး အေးဂျင့်များစွာဖြင့် ကိုင်တွယ်ပါ။ |
| AI အေးဂျင့်သည် ဆက်လက် loop များတွင် ရောက်နေခြင်း | - အေးဂျင့်သည် လုပ်ငန်းစဉ်ကို ရပ်တန့်ရမည့် အခြေအနေများကို သတ်မှတ်ပါ။<br>- Reasoning tasks အတွက် အထူးပြုထားသော မော်ဒယ်ကို အသုံးပြုပါ။ |
| AI အေးဂျင့် tool call များကောင်းစွာ မလုပ်ဆောင်နိုင်ခြင်း | - အေးဂျင့်စနစ်အပြင်တွင် tool output ကို စမ်းသပ်ပြီး အတည်ပြုပါ။ |

- သတ်မှတ်ထားသော parameters, prompts, နှင့် tools အမည်များကို ပြင်ဆင်ပါ။  
| Multi-Agent system သာမန်မလုပ်ဆောင်နိုင်ခြင်း | - တစ်ဦးချင်း agent များကို ပေးထားသော prompts များကို ပြင်ဆင်ပြီး တစ်ခုနှင့်တစ်ခု မတူညီအောင် သေချာစေပါ။<br>- "routing" သို့မဟုတ် controller agent ကို အသုံးပြု၍ အဆင့်လိုက်စနစ်တစ်ခု တည်ဆောက်ပြီး မည်သည့် agent သင့်တော်သည်ကို ဆုံးဖြတ်ပါ။ |

ဤပြဿနာများအများစုကို observability ရှိနေပါက ပိုမိုထိရောက်စွာ ရှာဖွေနိုင်ပါသည်။ အရင်က ပြောခဲ့သော traces နှင့် metrics များက agent workflow ၏ မည်သည့်နေရာတွင် ပြဿနာများ ဖြစ်ပေါ်သည်ကို တိကျစွာ ဖော်ထုတ်ပေးပြီး debugging နှင့် optimization ကို ပိုမိုထိရောက်စေပါသည်။

## ကုန်ကျစရိတ်ကို စီမံခြင်း

AI agents များကို production တွင် အသုံးချရာတွင် ကုန်ကျစရိတ်ကို စီမံရန် အောက်ပါနည်းလမ်းများကို အသုံးပြုနိုင်ပါသည်-

**အသေးစား Models အသုံးပြုခြင်း:** Small Language Models (SLMs) သည် agentic use-cases အချို့တွင် ကောင်းစွာ လုပ်ဆောင်နိုင်ပြီး ကုန်ကျစရိတ်ကို အလွန်လျော့ချနိုင်ပါသည်။ အရင်က ပြောခဲ့သည့်အတိုင်း performance ကို ကြီးမားသော models များနှင့် နှိုင်းယှဉ်ပြီး သင့် use case တွင် SLM က ဘယ်လောက်ကောင်းကောင်းလုပ်ဆောင်နိုင်မည်ကို နားလည်ရန် အကဲဖြတ်စနစ်တစ်ခု တည်ဆောက်ပါ။ SLM များကို intent classification သို့မဟုတ် parameter extraction ကဲ့သို့ ရိုးရှင်းသောအလုပ်များအတွက် အသုံးပြုပါ၊ reasoning အလွန်ရှုပ်ထွေးသောအလုပ်များအတွက် ကြီးမားသော models များကိုသာ အသုံးပြုပါ။

**Router Model အသုံးပြုခြင်း:** အမျိုးမျိုးသော models နှင့် sizes ကို အသုံးပြုသည့် နည်းလမ်းတစ်ခုဖြစ်သည်။ LLM/SLM သို့မဟုတ် serverless function ကို အသုံးပြု၍ complexity အပေါ်မူတည်ပြီး requests များကို သင့်တော်သော models များဆီသို့ route လုပ်ပါ။ ဤနည်းလမ်းသည် ကုန်ကျစရိတ်ကို လျော့ချစေသလို၊ သင့်တော်သော tasks များတွင် performance ကိုလည်း အာမခံပေးပါသည်။ ဥပမာအားဖြင့် ရိုးရှင်းသော queries များကို အသေးစား၊ မြန်ဆန်သော models များဆီသို့ route လုပ်ပြီး reasoning အလွန်ရှုပ်ထွေးသော tasks များအတွက်သာ ကြီးမားသော models များကို အသုံးပြုပါ။

**Responses များကို Caching လုပ်ခြင်း:** အများဆုံးဖြစ်သော requests နှင့် tasks များကို ရှာဖွေပြီး agentic system ကို မသွားမီ responses များကို ပေးခြင်းသည် တူညီသော requests များ၏ အရေအတွက်ကို လျော့ချရန် ကောင်းမွန်သောနည်းလမ်းတစ်ခုဖြစ်သည်။ AI models အခြေခံများကို အသုံးပြု၍ request တစ်ခုသည် cached requests များနှင့် ဘယ်လောက်တူညီသည်ကို ရှာဖွေသည့် flow တစ်ခုကိုတောင် တည်ဆောက်နိုင်ပါသည်။ ဤနည်းလမ်းသည် မကြာခဏမေးမြန်းသောမေးခွန်းများ သို့မဟုတ် ရိုးရှင်းသော workflows များအတွက် ကုန်ကျစရိတ်ကို အလွန်လျော့ချနိုင်ပါသည်။

## အကဲဖြတ်မှုကို လက်တွေ့အသုံးချခြင်း

ဤအပိုင်း၏ [ဥပမာ notebook](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) တွင် observability tools များကို အသုံးပြု၍ agent ကို စောင့်ကြည့်ခြင်းနှင့် အကဲဖြတ်ခြင်းကို လုပ်ဆောင်ပုံ ဥပမာများကို တွေ့ရပါမည်။

## အရင်စာရင်း

[Metacognition Design Pattern](../09-metacognition/README.md)

## နောက်စာရင်း

[MCP](../11-mcp/README.md)

**အကြောင်းကြားချက်**:  
ဤစာရွက်စာတမ်းကို AI ဘာသာပြန်ဝန်ဆောင်မှု [Co-op Translator](https://github.com/Azure/co-op-translator) ကို အသုံးပြု၍ ဘာသာပြန်ထားပါသည်။ ကျွန်ုပ်တို့သည် တိကျမှုအတွက် ကြိုးစားနေသော်လည်း၊ အလိုအလျောက် ဘာသာပြန်မှုများတွင် အမှားများ သို့မဟုတ် မမှန်ကန်မှုများ ပါဝင်နိုင်သည်ကို သတိပြုပါ။ မူရင်းစာရွက်စာတမ်းကို ၎င်း၏ မူလဘာသာစကားဖြင့် အာဏာတရ အရင်းအမြစ်အဖြစ် သတ်မှတ်သင့်ပါသည်။ အရေးကြီးသော အချက်အလက်များအတွက် လူ့ဘာသာပြန်ပညာရှင်များမှ ပရော်ဖက်ရှင်နယ် ဘာသာပြန်မှုကို အကြံပြုပါသည်။ ဤဘာသာပြန်မှုကို အသုံးပြုခြင်းမှ ဖြစ်ပေါ်လာသော အလွဲအမှားများ သို့မဟုတ် အနားလွဲမှုများအတွက် ကျွန်ုပ်တို့သည် တာဝန်မယူပါ။