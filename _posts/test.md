---
layout: post
title: 测试
date: 2020-06-06
tag: 测试
---

# 这是一个测试帖
## 代码测试

```
def __init__(self, output_size, d_model=256, attention_heads=4, linear_units=2048, num_blocks=6, pos_dropout_rate=0.0, 
                 slf_attn_dropout_rate=0.0, src_attn_dropout_rate=0.0, ffn_dropout_rate=0.0, residual_dropout_rate=0.1,
                 activation='relu', normalize_before=True, concat_after=False, share_embedding=False):
        super(TransformerDecoder, self).__init__()

        self.normalize_before = normalize_before

        self.embedding = torch.nn.Embedding(output_size, d_model)
        self.pos_encoding = PositionalEncoding(d_model, pos_dropout_rate)

        self.blocks = nn.ModuleList([
            TransformerDecoderLayer(attention_heads, d_model, linear_units, slf_attn_dropout_rate, src_attn_dropout_rate,
                                    ffn_dropout_rate, residual_dropout_rate, normalize_before=normalize_before, concat_after=concat_after,
                                    activation=activation) for _ in range(num_blocks)
        ])

```

