# GPT-Integration-with-AirTable
Airtable + GPT-3 GPT-3 Prompt GPT-3 Prompt to API Details Integrate GPT-3 API Into Airtable

console.log(`Hello, ${base.name}!`);
let inputConfig = input.config();
// console.log(`The Question is : ${inputConfig.Question}`);

let table = base.getTable('all questions');

// console.log("Table: ",table)

let prompt = `Pick the correct answer from the given four answers (A, B, C) for the following question:\nQuestion: A 60 years old patient come with sudden onset of upper abdominal pain after a few bouts of vomiting. Examination confirmed a sick patient with tenderness in epigastrium and supraclavicular subconscious emphysema. What’s the diagnosis? \nOption A: Esophagitis\nOption B: Acute gastritis\nOption C: Perforated peptic ulcer\n\nAnswer: C Perforated peptic ulcer \n\n##\n\nQuestion: Pt came with several episodes of vomiting of blood he is healthy not have\nany disease, not a smoker not alcoholic not on any medication? in other recall many bouts of hematemesis after prolonged vomiting, Dx?\nOption A: PUD\nOption B: Mallory weiss tear\nOption C: Varicose veins\n\nAnswer: B Mallory weiss tear.\n\n##\n\nQuestion: ${inputConfig.Question}  \n\nOption A: ${inputConfig.A}\nOption B: ${inputConfig.B}\nOption C: ${inputConfig.C}\n\nAnswer:  \n##\n`
console.log("Prompt: ",prompt)
let response = await fetch("https://api.openai.com/v1/completions", {
    body: JSON.stringify({
    "model": "text-davinci-003",
    "prompt": prompt,
    "max_tokens": 256,
    "top_p": 1,
    "frequency_penalty": 0,
    "presence_penalty": 0
  }),
    headers: {
      Authorization: "Bearer ",
      "Content-Type": "application/json"
    },
    method: "POST"
  })
console.log(response)

let data = await response.json()
console.log(data['choices'])
let returndata = data['choices'][0]['text'].trim()
console.log(returndata)
if (returndata){
    console.log('GPT-3 Replay: '+returndata)
    await table.updateRecordAsync(inputConfig.id, {
        "Correct Choice-GPT" : returndata
    })
}
