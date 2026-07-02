/api/resource:
  post:
    summary: [One-line description]
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/CreateInput'
    responses:
      '201':
        description: Resource created
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Resource'
      '422':
        description: Validation error
