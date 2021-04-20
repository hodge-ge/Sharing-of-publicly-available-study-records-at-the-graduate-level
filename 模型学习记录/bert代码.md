 ~~~
def __init__(self, config):
    super(BertForTokenClassification, self).__init__(config)
    self.num_labels = config.num_labels #假设有10个标签

    self.bert = BertModel(config) #hidden_size维度为768
    self.dropout = nn.Dropout(config.hidden_dropout_prob)
    self.classifier = nn.Linear(config.hidden_size, config.num_labels)#该线性层为768*10

    self.init_weights()

 ~~~

~~~
def forward(
        self,
        input_ids=None,#(batch_size, sequence_length)
        attention_mask=None,#(batch_size, sequence_length)
        token_type_ids=None, # (batch_size, sequence_length)
        position_ids=None,#(batch_size, sequence_length)
        head_mask=None,#(num_heads,) or (num_layers, num_heads)
        inputs_embeds=None,#(batch_size, sequence_length, hidden_size)
        labels=None,#(batch_size, sequence_length)
    ):
        
      outputs = self.bert(
            input_ids,#(batch_size, sequence_length)
            attention_mask=attention_mask,#(batch_size, sequence_length)
            token_type_ids=token_type_ids, # (batch_size, sequence_length)
            position_ids=position_ids,#(batch_size, sequence_length)
            head_mask=head_mask,#(num_heads,) or (num_layers, num_heads)
            inputs_embeds=inputs_embeds,#(batch_size, sequence_length, hidden_size)   64*128*768
        )#原则上用到前四个参数即可，也可直接取第几层输入，有论文表示第3层输出用于命名实体识别效果最好

        sequence_output = outputs[0] #假设输入为64*128*768
	    '''
	     loss:(1,)
	     logits :(batch_size, sequence_length, config.num_labels) (64,128,10)
	     hidden_state: (batch_size, sequence_length, hidden_size)
	     attention: (batch_size, num_heads, sequence_length, sequence_length)
	    '''
        sequence_output = self.dropout(sequence_output) 
        logits = self.classifier(sequence_output) # （64*128*768） * （768*10） = 64*128*10

        outputs = (logits,) + outputs[2:]  # add hidden states and attention if they are here

        if labels is not None: #给labels是训练
            loss_fct = CrossEntropyLoss() #交叉熵损失函数
            # Only keep active parts of the loss
            if attention_mask is not None:
                active_loss = attention_mask.view(-1) == 1 #先把attention_mask展开，其中等于1为True，等于0为False，维度 64*128 = 8192
                active_logits = logits.view(-1, self.num_labels) #把logits展开，维度为8192*10
                active_labels = torch.where(
                    active_loss, labels.view(-1), torch.tensor(loss_fct.ignore_index).type_as(labels)
                )#where三个参数，第一个为判断条件，第二个是复合条件设置值，第三个不符合条件设置值。loss_fct.ignore_index默认为-100，可以在初始化CrossEntropyLoss(ignore_index=-1)修改
                loss = loss_fct(active_logits, active_labels)
            else: #不给labels是预测
                loss = loss_fct(logits.view(-1, self.num_labels), labels.view(-1))
            outputs = (loss,) + outputs

        return outputs  # (loss), scores, (hidden_states), (attentions)

~~~

