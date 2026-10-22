порядок моих действий:
1. экспортируем слой со словами, я брала только слой информанта
2. столицы токенов я размечала не через mystem: я попыталась запускать через свой питон, но, насколько я поняла, проблема в том, что у меня макбук на arm-архитектуре, а на нем он не поддерживается. поэтому я нашла аналог pymorphy3. помимо него использовала также pandas для более удобной работы с созданием новых таблиц.
собственно код:
import pandas as pd
import pymorphy3
morph = pymorphy3.MorphAnalyzer()
df = pd.read_csv('words_export.txt', sep='\t', names=['Begin', 'End', 'Token'])
lemmas = []
morphs = []
for word in df['Token']:
    word_str = str(word).strip()
    if not word_str or pd.isna(word):
        lemmas.append("")
        morphs.append("")
        continue
    parsed = morph.parse(word_str)
    if parsed:
        first_match = parsed[0]
        lemma = first_match.normal_form
        grammemes = str(first_match.tag)
    else:
        lemma = word_str
        grammemes = 'UNKNOWN'
    lemmas.append(lemma)
    morphs.append(grammemes)
df['Lemma'] = lemmas
df['Morph'] = morphs
df[['Begin', 'End', 'Lemma']].to_csv('lemmas_import.txt', sep='\t', index=False, header=False)
df[['Begin', 'End', 'Morph']].to_csv('morphs_import.txt', sep='\t', index=False, header=False)

3. получившиеся файлы импортируем в елан.
4. наискосок смотрим результаты. вообще, довольно все неплохо он определил, на первый взгляд я ошибок не нашла. зато нашла ошибки у себя в разметке частей речи, позор мне! машина размечает лучше меня. докатились. :(
5. я также попыталась сделать стереотип для слоев, но елан не дал почему-то.

это был интересный опыт
