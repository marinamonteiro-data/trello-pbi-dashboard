let
    Fonte = Json.Document(
        Web.Contents(
            "https://api.trello.com",
            [
                RelativePath = "1/boards/SEU_QUADRO/lists",
                Query = [
                    key = "SUA_CHAVE",
                    token = "SEU_TOKEN"
                ]
            ]
        )
    ),

    #"Convertido para Tabela" = Table.FromList(Fonte, Splitter.SplitByNothing(), null, null, ExtraValues.Error),

    #"Listas Expandido" = Table.ExpandRecordColumn(
        #"Convertido para Tabela",
        "Column1",
        {"id","name"},
        {"idList","NomeLista"}
    ),

    #"Buscar Cards" = Table.AddColumn(
        #"Listas Expandido",
        "Cards",
        each Json.Document(
            Web.Contents(
                "https://api.trello.com",
                [
                    RelativePath = "1/lists/" & [idList] & "/cards",
                    Query = [
                        key = "SUA_CHAVE",
                        token = "SEU_TOKEN"
                    ]
                ]
            )
        )
    ),

    #"Cards Expandido" = Table.ExpandListColumn(#"Buscar Cards", "Cards"),
    #"Cards Expandido1" = Table.ExpandRecordColumn(#"Cards Expandido", "Cards", {"id", "agent", "badges", "checkItemStates", "closed", "dueComplete", "dateLastActivity", "desc", "descData", "due", "dueReminder", "email", "idBoard", "idChecklists", "idList", "idMembers", "idMembersVoted", "idShort", "idAttachmentCover", "labels", "idLabels", "manualCoverAttachment", "name", "nodeId", "pinned", "pos", "shortLink", "shortUrl", "start", "subscribed", "url", "cover", "isTemplate", "cardRole", "mirrorSourceId"}, {"id", "agent", "badges", "checkItemStates", "closed", "dueComplete", "dateLastActivity", "desc", "descData", "due", "dueReminder", "email", "idBoard", "idChecklists", "idList.1", "idMembers", "idMembersVoted", "idShort", "idAttachmentCover", "labels", "idLabels", "manualCoverAttachment", "name", "nodeId", "pinned", "pos", "shortLink", "shortUrl", "start", "subscribed", "url", "cover", "isTemplate", "cardRole", "mirrorSourceId"}),
    #"Função Personalizada Invocada" = Table.AddColumn(#"Cards Expandido1", "Checklist", each fn_Checklists_Card([id], [name])),
    #"Checklist Expandido" = Table.ExpandTableColumn(#"Função Personalizada Invocada", "Checklist", {"Item_Checklist", "Estado", "id_card"}, {"Item_Checklist", "Estado", "id_card"}),
    #"Coluna Duplicada" = Table.DuplicateColumn(#"Checklist Expandido", "dateLastActivity", "dateLastActivity - Copiar"),
    #"Tipo Alterado" = Table.TransformColumnTypes(#"Coluna Duplicada",{{"dateLastActivity - Copiar", type datetime}, {"due", type datetime}}),
    #"Duplicatas Removidas" = Table.Distinct(#"Tipo Alterado", {"id_card"})
in
    #"Duplicatas Removidas"
