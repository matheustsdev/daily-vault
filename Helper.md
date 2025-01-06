GET /topcon-integrator-desktop_dispatch-prd/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase": {
            "message": "nfe_nNF = 23"
          }
        }
      ]
    }
  }
}