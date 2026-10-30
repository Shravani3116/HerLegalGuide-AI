
const items = $input.all();

// Get the original message from Telegram
const question = String(
  $('Telegram Trigger').first().json.message?.text || ''
).trim();

const normalizedQuestion = question.toLowerCase();

// 1. Handle greetings before searching the dataset
const greetingPattern =
  /^(hi+|hello+|hey+|hii+|hiii+|good morning|good afternoon|good evening|good night|how are you)[!.,?\s]*$/i;

if (greetingPattern.test(question)) {
  return [
    {
      json: {
        reply:
          "Hello! 👋 Welcome to HerLegalGuide AI. I can help you understand women's legal rights in India. You can ask me about dowry, domestic violence, workplace harassment, marriage rights, or other legal issues."
      }
    }
  ];
}

// 2. Handle an empty message
if (!question) {
  return [
    {
      json: {
        reply: "Please enter a question about women's legal rights in India."
      }
    }
  ];
}

// 3. Read the dataset rows from Extract from File
const dataset = items
  .map(item => item.json)
  .filter(row => row && typeof row === 'object');

// 4. Prepare text for searching
function normalize(value) {
  return String(value ?? '')
    .toLowerCase()
    .replace(/[^a-z0-9\s]/g, ' ')
    .replace(/\s+/g, ' ')
    .trim();
}

// Common legal terms and related words
const synonyms = {
  dowry: [
    'dowry',
    'dowry demand',
    'dowry demands',
    'dowry harassment',
    'dowry prohibition act',
    'dowry related',
    'bride price'
  ],
  'domestic violence': [
    'domestic violence',
    'violence at home',
    'abuse by husband',
    'husband abuse',
    'marital abuse',
    'protection of women from domestic violence act'
  ],
  'workplace harassment': [
    'workplace harassment',
    'sexual harassment at work',
    'harassment at workplace',
    'posh act',
    'sexual harassment'
  ],
  'equal rights': [
    'equal rights',
    'gender equality',
    'equality',
    'discrimination against women'
  ],
  'child marriage': [
    'child marriage',
    'underage marriage',
    'minor marriage'
  ],
  'maternity benefits': [
    'maternity benefits',
    'maternity leave',
    'pregnancy benefits'
  ]
};

// 5. Expand the question with relevant search terms
let searchText = normalize(question);

for (const [topic, terms] of Object.entries(synonyms)) {
  if (terms.some(term => searchText.includes(normalize(term)))) {
    searchText += ' ' + normalize(topic) + ' ' +
      terms.map(normalize).join(' ');
  }
}

const questionWords = [
  ...new Set(
    searchText
      .split(' ')
      .filter(word => word.length > 2)
  )
];

// 6. Score each dataset row against the question
const scoredRows = dataset.map(row => {
  const category = normalize(row.category);
  const lawName = normalize(row.law_name);
  const rowQuestion = normalize(row.question);
  const answer = normalize(row.answer);

  const searchableText =
    `${category} ${lawName} ${rowQuestion} ${answer}`;

  let score = 0;

  for (const word of questionWords) {
    if (lawName.includes(word)) score += 5;
    if (rowQuestion.includes(word)) score += 4;
    if (category.includes(word)) score += 3;
    if (answer.includes(word)) score += 1;
  }

  // Give extra weight to direct topic matches
  if (
    searchText.includes('dowry') &&
    `${lawName} ${rowQuestion} ${category}`.includes('dowry')
  ) {
    score += 12;
  }

  if (
    searchText.includes('domestic violence') &&
    `${lawName} ${rowQuestion} ${category}`.includes('domestic violence')
  ) {
    score += 12;
  }

  if (
    searchText.includes('workplace harassment') &&
    `${lawName} ${rowQuestion} ${category}`.includes('harassment')
  ) {
    score += 12;
  }

  return { row, score, searchableText };
});

// 7. Select the best matching rows
scoredRows.sort((a, b) => b.score - a.score);

const matches = scoredRows
  .filter(result => result.score >= 5)
  .slice(0, 3);

// 8. Prepare information for the AI Agent
if (matches.length > 0) {
  const reply = matches.map((result, index) => {
    const row = result.row;

    return [
      `Dataset result ${index + 1}:`,
      `Category: ${row.category || 'Not specified'}`,
      `Question: ${row.question || 'Not specified'}`,
      `Law: ${row.law_name || 'Not specified'}`,
      `Answer: ${row.answer || 'Not specified'}`,
      `Official source: ${row.source_url || 'Not provided'}`,
      `Disclaimer: ${row.disclaimer || 'General legal information only.'}`
    ].join('\n');
  }).join('\n\n');

  return [
    {
      json: {
        reply,
        originalQuestion: question,
        matchFound: true
      }
    }
  ];
}

// 9. No relevant dataset match
return [
  {
    json: {
      reply:
        "I could not find a reliable matching answer in my dataset. Please try asking about a specific law or legal issue, or check an official government source or consult a qualified legal-aid lawyer.",
      originalQuestion: question,
      matchFound: false
    }
  }
];
