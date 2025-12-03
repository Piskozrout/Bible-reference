<%*
  const input = await tp.system.prompt("Zadej Biblickou knihu: (např. 1moj 1:1)");
  if (!input) {
  tR = "⛔️ Nebyl zadán žádný text!";
} else {
  const parts = input.match(/^([\p{L}\p{N}]+)\s+(\d+):([\d,\-\s]+)$/iu);

  if (!parts) {
    tR = "❌ Neplatný formát";
  } else {
    const [_, book, chapter, verse] = parts;
    const bookMap = {
      "1moj": "1",
      "2moj": "2",
      "3moj": "3",
      "4moj": "4",
      "5moj": "5",
      "joz": "6", "jozue": "6",
      "sd": "7", "soudci": "7", "soud": "7",
      "rut": "8",
      "1sam": "9", "1samuel": "9",
      "2sam": "10", "2samuel": "10",
      "1kr": "11", "1kral": "11",
      "2kr": "12", "2kral": "12",
      "1par": "13", "1para": "13",
      "2par": "14", "2para": "14",
      "ezr": "15", "ezra": "15",
      "neh": "16", "nehe": "16", "nehem": "16",
      "es": "17", "est": "17", "ester": "17",
      "job": "18",
	  "za": "19", "zal": "19", "zalm": "19", "zalmy": "19",
      "pr": "20", "pri": "20", "pris": "20", "prisl": "20", "prislovi": "20",
      "ka": "21", "kaz": "21", "kazat": "21", "kazatel": "21",
      "pi": "22", "pis": "22", "pisne": "22", "salamoun": "22",
      "iz": "23", "iza": "23", "izaj": "23", "izajas": "23",
      "je": "24", "jer": "24", "jere": "24", "jerem": "24", "jeremjas": "24",
      "nar": "25", "narky": "25",
      "eze": "26", "ezek": "26", "ezekiel": "26",
      "dan": "27", "daniel": "27",
      "oz": "28", "ozeas": "28",
      "joel": "29",
      "am": "30", "amos": "30",
      "ob": "31", "oba": "31", "obad": "31", "obadjas": "31",
      "jon": "32", "jonas": "32",
      "mi": "33", "mich": "33", "micheas": "33",
      "nah": "34", "nahum": "34",
      "ha": "35", "hab": "35", "habakuk": "35",
      "se": "36", "sef": "36", "sefan": "36", "sefanjas": "36",
      "ag": "37", "ageus": "37",
      "ze": "38", "zech": "38", "zecharjas": "38",
      "mal": "39", "mala": "39", "malach": "39", "malachias": "39",
      "mat": "40", "mt": "40", "matous": "40",
      "mar": "41", "mk": "41", "marek": "41",
      "luk": "42", "lk": "42", "lukas": "42",
      "jan": "43", "jn": "43",
      "sk": "44", "skut": "44", "skutky": "44",
      "ri": "45", "rim": "45", "rimanum": "45",
      "1kor": "46",
      "2kor": "47",
      "ga": "48", "gal": "48", "galatanum": "48",
      "ef": "49", "efe": "49", "efez": "49", "efezanum": "49",
      "fil": "50", "filip": "50", "filipanum": "50",
      "kol": "51", "kolo": "51", "kolos": "51", "kolosanum": "51",
      "1tes": "52",
      "2tes": "53",
      "1tim": "54",
      "2tim": "55",
      "tit": "56", "titovi": "56",
      "flm": "57", "file": "57", "filemonovi": "57",
      "he": "58", "heb": "58", "hebrej": "58", "hebrejcum": "58",
      "jak": "59", "jakub": "59",
      "1pet": "60", "1petra": "60", "1pe": "60", "1petr": "60",
      "2pet": "61", "2petra": "61", "2pe": "61", "2petr": "61",
      "1jan": "62", "1jana": "62",
      "2jan": "63", "2jana": "63",
      "3jan": "64", "3jana": "64",
      "ju": "65", "jud": "65", "juda": "65",
      "zj": "66", "zjev": "66", "zjeveni": "66"
    };

    const bookNameMap = {
      "1": "1. Mojžíšova",
      "2": "2. Mojžíšova",
      "3": "3. Mojžíšova",
      "4": "4. Mojžíšova",
      "5": "5. Mojžíšova",
      "6": "Jozue",
      "7": "Soudci",
      "8": "Rut",
      "9": "1. Samuelova",
      "10": "2. Samuelova",
      "11": "1. Královská",
      "12": "2. Královská",
      "13": "1. Paralipomenon",
      "14": "2. Paralipomenon",
      "15": "Ezra",
      "16": "Nehemjáš",
      "17": "Ester",
      "18": "Job",
      "19": "Žalm",
      "20": "Přísloví",
      "21": "Kazatel",
      "22": "Šalomounova píseň",
      "23": "Izajáš",
      "24": "Jeremjáš",
      "25": "Nářky",
      "26": "Ezekiel",
      "27": "Daniel",
      "28": "Ozeáš",
      "29": "Joel",
      "30": "Amos",
      "31": "Obadjáš",
      "32": "Jonáš",
      "33": "Micheáš",
      "34": "Nahum",
      "35": "Habakuk",
      "36": "Sefanjáš",
      "37": "Ageus",
      "38": "Zecharjáš",
      "39": "Malachiáš",
      "40": "Matouš",
      "41": "Marek",
      "42": "Lukáš",
      "43": "Jan",
      "44": "Skutky",
      "45": "Římanům",
      "46": "1. Korinťanům",
      "47": "2. Korinťanům",
      "48": "Galaťanům",
      "49": "Efezanům",
      "50": "Filipanům",
      "51": "Kolosanům",
      "52": "1. Tesaloničanům",
      "53": "2. Tesaloničanům",
      "54": "1. Timoteovi",
      "55": "2. Timoteovi",
      "56": "Titovi",
      "57": "Filemonovi",
      "58": "Hebrejcům",
      "59": "Jakub",
      "60": "1. Petra",
      "61": "2. Petra",
      "62": "1. Jana",
      "63": "2. Jana",
      "64": "3. Jana",
      "65": "Juda",
      "66": "Zjevení"
    };

    const newbook = book.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "");
    const bookCode = bookMap[newbook];

    if (!bookCode) {
      tR = `❌ Neznámá kniha: ${book}`;
    } else {
      const chapterCode = chapter.padStart(3, "0");
      const output = [];

      const verseParts = verse.split(",").map(v => v.trim());
      for (let i = 0; i < verseParts.length; i++) {
        const part = verseParts[i];

        if (part.includes("-")) {
          const [from, to] = part.split("-").map(v => v.trim().padStart(3, "0"));
          output.push({
        code: `${bookCode}${chapterCode}${String(from).padStart(3, "0")}-${bookCode}${chapterCode}${String(to).padStart(3, "0")}`,
        label: `${part}`
          });
        } else {
          const current = parseInt(part);
          const next = parseInt(verseParts[i + 1]);

          if (!isNaN(next) && next === current + 1) {
            output.push({
              code: `${bookCode}${chapterCode}${String(current).padStart(3, "0")}-${bookCode}${chapterCode}${String(next).padStart(3, "0")}`,
              label: `${current}, ${next}`
            });
            i++;
          } else {
            output.push({
              code: `${bookCode}${chapterCode}${String(current).padStart(3, "0")}`,
              label: `${current}`
            });
          }
        }
      }

      tR = output.map((item, i) => {
      const url = `https://www.jw.org/finder?bible=${item.code}&pub=nwt&wtlocale=B`;
      return i === 0
        ? `[${bookNameMap[bookCode]} ${chapter}:${item.label}](${url})`
        : `[, ${item.label}](${url})`;
      }).join("");
    }
  }
}
tR;
%>
