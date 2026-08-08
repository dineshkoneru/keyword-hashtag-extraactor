import os
import spacy
import gradio as gr
from collections import Counter

nlp = spacy.load("en_core_web_sm")


def generate_hashtags(text, num_hashtags=10):
    doc = nlp(text)

    keywords = []

    for token in doc:
        if token.pos_ in ["NOUN", "PROPN", "ADJ"]:
            if not token.is_stop and not token.is_punct:
                keywords.append(token.lemma_.lower())

    keyword_freq = Counter(keywords)
    top_keywords = keyword_freq.most_common(num_hashtags)

    hashtags = ["#" + word for word, count in top_keywords]

    return " ".join(hashtags)


with gr.Blocks(
    theme=gr.themes.Soft(),
    title="AI Hashtag Generator"
) as demo:

    gr.Markdown("""
    # 🤖 AI Hashtag Generator
    ### Generate Smart Hashtags using Natural Language Processing
    """)

    input_text = gr.Textbox(
        label="Enter Paragraph",
        placeholder="Paste your paragraph here...",
        lines=8
    )

    generate_btn = gr.Button("Generate Hashtags")

    output = gr.Textbox(
        label="Generated Hashtags",
        lines=4
    )

    clear_btn = gr.Button("Clear")

    generate_btn.click(
        fn=generate_hashtags,
        inputs=input_text,
        outputs=output
    )

    clear_btn.click(
        fn=lambda: ("", ""),
        outputs=[input_text, output]
    )


demo.launch(
    server_name="0.0.0.0",
    server_port=int(os.environ.get("PORT", 10000))
)
