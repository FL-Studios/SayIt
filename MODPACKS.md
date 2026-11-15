# SayIt for modpacks: your own sentences

From **SayIt 1.3.0**, a modpack, datapack or command block can ask players a sentence of its own, for moments SayIt does
not know about: boarding a Create train, entering a custom dimension, finishing a quest. SayIt shows the sentence,
listens and scores it like its own sentences (XP on success, never a penalty).

## 1. Add the texts

Sentences are ordinary language keys, `sayitenglish.s.<name>.<n>`, numbered from `0`. Put them in a resource pack
(or KubeJS `assets`) under `assets/sayitenglish/lang/`, one file per language:

`en_us.json`
```json
{
  "sayitenglish.s.train.0": "I am riding the train.",
  "sayitenglish.s.train.1": "The next stop is %s."
}
```

`es_es.json`
```json
{
  "sayitenglish.s.train.0": "Voy en tren.",
  "sayitenglish.s.train.1": "La próxima parada es %s."
}
```

- SayIt picks one of the numbered texts at random.
- `%s` is replaced by the item you pass to the command, in the learned language. When you ask without an item, only
  the texts without `%s` are used; if every text has `%s`, the player is not asked.
- **A player is only asked in a language that has the text.** Someone learning Japanese is not asked a sentence that
  only exists in `en_us.json` (nothing is kept or punished, the question is just skipped).
- Keep sentences short and natural: they are spoken out loud.

## 2. Ask it

```
/sayitenglish ask <players> <name> [item]
```

Examples:
```
/sayitenglish ask @p train
/sayitenglish ask @a[distance=..10] train minecraft:rail
execute as @a[tag=on_train] run sayitenglish ask @s train
```

- Needs permission level 2 (operators, command blocks, functions, the console).
- `<name>`: letters `a-z`, digits, `_` and `.`, for example `train` or `mypack.quest_done`.
- The command returns how many players were asked, so a command block or function can check it.
- A player is **not** asked (the command says why) when they are answering another question, have paused SayIt,
  have not chosen a language yet, or play with a SayIt older than 1.3.0.

There is no cooldown on this command: you decide how often to ask. Once every few minutes per player is plenty.
