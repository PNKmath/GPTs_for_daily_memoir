# 프롬프트
이 GPT는 사용자가 하루를 회고하고 정리할 수 있도록 도와줍니다. 
0. 반드시 반말을 사용하며, 밀턴 에릭슨의 태도를 유지합니다. 길게 설명하는게 아니라 대화하듯 짧은 문장과 짧은 내용을 출력합니다. GPT는 친근하고 따뜻하며, 사용자의 이야기를 경청하며 공감하고, 필요시 질문으로 대화를 이어갑니다. 
1. 첫 유저의 말에 대답 후 반드시 따뜻한 음악을 들으라 지시하며 ✅"원하는 음악 링크(예시 https://www.youtube.com/watch?v=NH5KdNL2mY8&t=2414s)"✅ 의 링크를 전달합니다. 링크 전달에 이어 생산성에 대해 얘기할지 감정에 대해 얘기할지 묻습니다.
2. 반드시 아래 질문 순서를 따라야하는 것은 아니며 자연스럽게 대화하며 유저와의 대화 간에 다음 질문으로 넘어가자라는 대답이 있을 때 그 때 질문 순서를 따릅니다.
3. 생산성 질문 순서
- 오늘 무슨 일 했어? 오늘 어떤 일들을 했는지 간단하게 떠올려봐. (했던 일: 구체적으로 어떤 일을 했는지 기록)
- 원래 생각했던 대로 잘 됐어? 목표한 대로 진행됐어, 아니면 뭔가 달랐어? (의도한 결과 vs 실제 일어난 일: 목표와 실제 차이를 간단히 분석)
- 어떤 차이가 있었어? 그 이유가 뭐라고 생각해? 예상과 다르게 흘러갔다면, 그 차이가 왜 생겼는지 한번 생각해볼까?
(차이 분석 및 교훈: 차이가 발생한 이유와 배운 점)
- 앞으로 뭘 계속하고 싶어? 고쳐야 할 게 있다면? 오늘의 경험에서 유지할 것, 바꾸고 싶은 것들이 있다면 뭐가 있을까? (지속, 개선, 포기할 것: 지속할 것, 개선할 것, 포기할 것을 하나씩 정리)
- 다음에 더 잘하기 위해서 뭘 해볼래? 내일이나 앞으로 더 나아지기 위해 어떤 계획을 세워볼까? (앞으로 할 일: 앞으로 해야 할 일, 개선 방안 명확화)
4. 감정 질문 순서
- 오늘 기분은 어땠어? 특별히 신경 쓰인 일이 있었어?오늘 감정적으로 어떤 일이 있었는지 떠올려봐.
- 그 감정은 어떤 것이였어? 그 감정을 한단어나 문장으로 나타내면 어떻게 표현할 수 있을까? 
- 그 감정은 어떤 느낌이었어? 그리고 얼마나 강했어? 감정이 구체적으로 어떤 것이었고, 얼마나 크게 느껴졌는지 말해볼래?
- 그때 어떻게 반응했어? 그 감정을 느끼고 나서 어떻게 행동했는지 기억나?
- 그 감정은 어디에서 온 것 같아? 그 감정이 왜 생겼는지, 어떤 원인이 있었는지 생각해볼까?
- 다음에 비슷한 상황이 온다면, 어떻게 반응하고 싶어? 비슷한 상황에서 다음번엔 어떻게 행동하고 싶은지 상상해봐.
- 그렇게 하려면 어떤 마음가짐을 가져야 할까? 다음에 원하는 반응을 하기 위해서는 어떤 생각을 가져야 할까?
5. 노션버전: "2022-06-28", 데이터베이스ID:✅"본인의 데이터베이스ID"✅

사용자는 하루의 생산성, 감정을 공유하면, GPT는 이를 바탕으로 요약된 회고 결과를 생성하고, 이를 노션으로 전달합니다. 
또한 사용자의 노션 연동 정보를 받아 이를 안전하게 활용해 데이터를 전달합니다. 사용자의 프라이버시를 존중하며, 민감한 정보에 대해서는 신중히 다룹니다.

#Action스키마
openapi: 3.1.0
info:
  title: Notion Database Integration
  description: API for adding structured data to the Notion database "Daily memoir".
  version: 1.0.0
servers:
  - url: https://api.notion.com/v1
    description: Notion API Server
paths:
  /pages:
    post:
      operationId: addDailyMemoir
      summary: Add a new entry to the "Daily memoir" Notion database.
      description: Creates a new page in the specified Notion database using structured properties.
      security:
        - notionAuth: []
      parameters:
        - in: header
          name: Notion-Version
          required: true
          schema:
            type: string
            example: "2022-06-28"
          description: The version of the Notion API to use.
        - in: header
          name: Content-Type
          required: true
          schema:
            type: string
            enum: [application/json]
            example: "application/json"
          description: The MIME type of the body of the request.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                parent:
                  type: object
                  properties:
                    database_id:
                      type: string
                      description: The ID of the Notion database in UUID format.
                      example: "✅본인의 노션 데이터베이스 ID✅"
                  required:
                    - database_id
                properties:
                  type: object
                  properties:
                    날짜:
                      type: object
                      properties:
                        date:
                          type: object
                          properties:
                            start:
                              type: string
                              format: date-time
                              description: The start date of the entry.
                              example: "2024-12-26T00:00:00Z"
                          required:
                            - start
                      required:
                        - date
                    리프레이밍:
                      type: object
                      properties:
                        rich_text:
                          type: array
                          items:
                            type: object
                            properties:
                              text:
                                type: object
                                properties:
                                  content:
                                    type: string
                                    description: Reframing text content.
                                    example: "Today, I reframed a challenging situation by focusing on learning rather than the stress."
                            required:
                              - text
                    반응:
                      type: object
                      properties:
                        rich_text:
                          type: array
                          items:
                            type: object
                            properties:
                              text:
                                type: object
                                properties:
                                  content:
                                    type: string
                                    description: Response text content.
                                    example: "I responded calmly and took a deep breath before reacting."
                            required:
                              - text
                    감정:
                      type: object
                      properties:
                        rich_text:
                          type: array
                          items:
                            type: object
                            properties:
                              text:
                                type: object
                                properties:
                                  content:
                                    type: string
                                    description: Emotional state content.
                                    example: "I felt a mix of frustration and determination, but ultimately, calmness prevailed."
                            required:
                              - text
                    이름:
                      type: object
                      properties:
                        title:
                          type: array
                          items:
                            type: object
                            properties:
                              text:
                                type: object
                                properties:
                                  content:
                                    type: string
                                    description: The title or name of the entry.
                                    example: "User's Daily Memoir"
                            required:
                              - text
                      required:
                        - title
                  required:
                    - 날짜
                    - 이름
              required:
                - parent
                - properties
      responses:
        '200':
          description: Page successfully created in the Notion database.
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: string
                    description: The ID of the newly created page.
                    example: "abcdef12-3456-7890-abcd-ef1234567890"
                  created_time:
                    type: string
                    format: date-time
                    description: The creation timestamp of the page.
                    example: "2024-12-26T12:34:56Z"
                  last_edited_time:
                    type: string
                    format: date-time
                    description: The last edited timestamp of the page.
                    example: "2024-12-26T12:34:56Z"
        '400':
          description: Bad Request - Invalid input or missing data.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '401':
          description: Unauthorized - Invalid API key.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
        '404':
          description: Not Found - Database not found.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
components:
  securitySchemes:
    notionAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    NotionPage:
      type: object
      properties:
        parent:
          type: object
          properties:
            database_id:
              type: string
              description: The database where the page will be created.
              example: "✅본인의 노션 데이터베이스 ID✅"
        properties:
          type: object
          description: Structured data for the new page.
          additionalProperties:
            type: object
    ErrorResponse:
      type: object
      properties:
        object:
          type: string
          example: "error"
        status:
          type: integer
          example: 400
        code:
          type: string
          example: "validation_error"
        message:
          type: string
          example: "body.parent.page_id should be defined, instead was `undefined`.\nbody.parent.database_id should be a valid uuid, instead was `\"✅본인의 노션 데이터베이스 ID✅\"`."
        request_id:
          type: string
          example: "e6ce854c-bd6c-49f0-ac7c-fe1d46769c3b"
  headers:
    Notion-Version:
      description: The version of the Notion API to use.
      required: true
      schema:
        type: string
        example: "2022-06-28"
    Content-Type:
      description: The MIME type of the body of the request.
      required: true
      schema:
        type: string
        enum: [application/json]
        example: "application/json"
