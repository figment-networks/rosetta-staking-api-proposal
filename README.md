# Rosetta Staking API proposal

This is a draft for Rosetta Staking API proposal. 
The goal here is to provide generic data structures which could provide us answers for below questions:
* who staked and how much?
* who unstaked and how much?
* who received rewards?
* who was slashed?

This proposal consists of 3 parts:
* identifiers (similar to https://www.rosetta-api.org/docs/api_identifiers.html)
* objects (similar to https://www.rosetta-api.org/docs/api_objects.html)
* endpoints (similar to https://www.rosetta-api.org/docs/BlockApi.html)

## 1. Identifiers

- **Staking period identifier -** Identifies the staking period. The smallest period is one block, but for instance in Polkadot we also have sessions and eras

```json
{
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  }
}
```

- **Staking participant identifier -** Identifies the staking participants. This can be the operator of the network (validator, transcoder) or a staker

```json
{
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  }
}
```

## 2. Objects

- **Staking period -** If staking period is longer than one block, we display start and end of the period information

```json
{
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  },
  "start_height": 1,
  "end_height": 100,
  "start_time": 12435456,
  "end_time": 15435456,
  "metadata": {}
}
```

- **Staking participant** - Entity participating in staking. Can be a operator (validator), staker etc.

```json
{
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "role": "validator",
  "account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
      "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
      "metadata": {}
    },
    "metadata": {}
  },
  "identity": {
    "name": "Figment",
    "logo": "https://fiment.com/logo.png",
    "email": "validator@figment.com",
    "website": "http://figment.com"
  },
  "metadata": {}
}
```

- **Identity -** Identity of a staking participant

```json
{
  "name": "Figment",
  "logo": "https://fiment.com/logo.png",
  "email": "validator@figment.com",
  "website": "http://figment.com",
  "metadata": {}
}
```

- **Stake** - Staking activity when `staker` stakes given `amount` on `operator` (validator) in a given `staking period`

```json
{
  "staking_period": {
    "index": 100,
    "name": "epoch"
  },
  "staking_participants": [
    {
      "staking_participant_index": 0,
      "staking_activity_role": "operator",
      "staking_participant": {
        "staking_participant_identifier": {
          "address": "12zTG1vmvTqS3CP32TaHyAHWDGwDj334GpNCWRCDCPVGwXf4"
        }
      },
      "account": {
        "account_index": 0,
        "account_identifier": {
          "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
          "sub_account": {},
          "metadata": {}
        }
      },
      "metadata": {}
    },
    {
      "staking_participant_index": 1,
      "staking_activity_role": "staker",
      "staking_participant": {
        "staking_participant_identifier": {
          "address": "12zTG1vmvTqS3CP32TaHyAHWDGwDj334GpNCWRCDCPVGwXf4"
        }
      },
      "account": {
        "account_index": 0,
        "account_identifier": {
          "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
          "sub_account": {},
          "metadata": {}
        }
      },
      "metadata": {}
    }
  ],
  "amount": {
    "value": "1238089899992",
    "currency": {
      "symbol": "BTC",
      "decimals": 8,
      "metadata": {
        "Issuer": "Satoshi"
      }
    },
    "metadata": {}
  },
  "related_transactions": [
    {
      "transaction_identifier": {
        "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
      }
    }
  ],
  "metadata": {}
}
```

- **Unstake** - Staking activity when `staker` unstakes given `amount` from `operator` (validator) in a given `staking period`

```json
{
  "staking_period": {
    "index": 100,
    "name": "epoch"
  },
  "staking_participants": [
    {
      "staking_participant_index": 0,
      "staking_activity_role": "operator",
      "staking_participant": {
        "staking_participant_identifier": {
          "address": "12zTG1vmvTqS3CP32TaHyAHWDGwDj334GpNCWRCDCPVGwXf4"
        }
      },
      "account": {
        "account_index": 0,
        "account_identifier": {
          "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
          "sub_account": {},
          "metadata": {}
        }
      },
      "metadata": {}
    },
    {
      "staking_participant_index": 1,
      "staking_activity_role": "staker",
      "staking_participant": {
        "staking_participant_identifier": {
          "address": "12zTG1vmvTqS3CP32TaHyAHWDGwDj334GpNCWRCDCPVGwXf4"
        }
      },
      "account": {
        "account_index": 0,
        "account_identifier": {
          "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
          "sub_account": {},
          "metadata": {}
        }
      },
      "metadata": {}
    }
  ],
  "amount": {
    "value": "1238089899992",
    "currency": {
      "symbol": "BTC",
      "decimals": 8,
      "metadata": {
        "Issuer": "Satoshi"
      }
    },
    "metadata": {}
  },
  "related_transactions": [
    {
      "transaction_identifier": {
        "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
      }
    }
  ],
  "metadata": {}
}
```

- **Reward** - Staking activity when given `account` is rewarded with the `amount` at the end of `staking period`

```json
{
  "reward_identifier": {
    "hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "type": "staking_reward",
  "staking_period": {
    "index": 100,
    "name": "epoch"
  },
  "account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
      "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
      "metadata": {}
    },
    "metadata": {}
  },
  "amount": {
    "value": "1238089899992",
    "currency": {
      "symbol": "BTC",
      "decimals": 8,
      "metadata": {
        "Issuer": "Satoshi"
      }
    },
    "metadata": {}
  },
  "related_transactions": [
    {
      "transaction_identifier": {
        "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
      }
    }
  ],
  "metadata": {}
}
```

- **Slash** - Staking activity when given `account` which is being slashed by the `amount` at the end of `staking_period`

```json
{
  "slash_identifier": {
    "hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "type": "offline",
  "staking_period": {
    "index": 100,
    "name": "epoch"
  },
  "account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
      "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
      "metadata": {}
    },
    "metadata": {}
  },
  "amount": {
    "value": "1238089899992",
    "currency": {
      "symbol": "BTC",
      "decimals": 8,
      "metadata": {
        "Issuer": "Satoshi"
      }
    },
    "metadata": {}
  },
  "related_transactions": [
    {
      "transaction_identifier": {
        "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
      }
    }
  ],
  "metadata": {}
}
```

## 3. Endpoints

- **/staking-period -** returns information about staking period

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  }
}
```

Response:

```json
{
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  },
  "start_height": 1,
  "end_time": 100,
  "start_timestamp": 1582272577,
  "end_timestamp": 1582272577,
  "total_stake": 130303030,
  "metadata": {}
}
```

- /**operator -** returns information about operator

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  }
}
```

Response:

```json
{
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "role": "validator",
  "account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
      "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
      "metadata": {}
    },
    "metadata": {}
  },
  "identity": {
    "name": "Figment",
    "logo": "https://fiment.com/logo.png",
    "email": "validator@figment.com",
    "website": "http://figment.com"
  },
  "metadata": {}
}
```

- **/staker -** returns information about staker

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  }
}
```

Response:

```json
{
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
      "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
      "metadata": {}
    },
    "metadata": {}
  },
  "identity": {},
  "metadata": {}
}
```

- **/staking-period/operator** - returns operator (validator) information for given staking period.

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  }
}
```

Response:

```json
{
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  },
  "online": true,
  "commission": 10000,
  "accounts": [],
  "stakes": [],
  "unstakes": [],
  "rewards": [],
  "slashes": [],
  "metadata": {}
}
```

- **/staking-period/operator-pool -** returns information about operator (validators) in the operator pool for given staking period

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  }
}
```

Response:

```json
{
	"staking_period": {
		"index": 100,
		"name": "epoch"
	},
	"operators": [{
		"staking_participant_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
		},
		"staking_participant_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
		},
		"staking_participant_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
		},
		"staking_participant_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
		}
	}],
	"metadata": {}
}
```

- **/staking-period/staker -** returns information about staker for given staking period

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  },
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  }
}
```

Response:

```json
{
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  },
  "staking_participant_identifier": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
  },
  "accounts": [],
  "stakes": [],
  "unstakes": [],
  "rewards": [],
  "slashes": [],
  "metadata": {}
}
```

- **/staking-period/reward -** returns rewards paid for given staking period

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  }
}
```

Response:

```json
{
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  },
  "rewards": [
    {
      "reward_index": 0,
      "reward_identifier": {
        "hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
      },
      "type": "staking_reward",
      "staking_period": {
        "index": 100,
        "name": "epoch"
      },
      "account": {
        "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
        "sub_account": {
          "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "metadata": {}
        },
        "metadata": {}
      },
      "amount": {
        "value": "1238089899992",
        "currency": {
          "symbol": "BTC",
          "decimals": 8,
          "metadata": {
            "Issuer": "Satoshi"
          }
        },
        "metadata": {}
      },
      "related_transactions": [
        {
          "transaction_identifier": {
            "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
          }
        }
      ],
      "metadata": {}
    },
    {
      "reward_index": 1,
      "reward_identifier": {
        "hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
      },
      "type": "staking_reward",
      "staking_period": {
        "index": 100,
        "name": "epoch"
      },
      "account": {
        "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
        "sub_account": {
          "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "metadata": {}
        },
        "metadata": {}
      },
      "amount": {
        "value": "1238089899992",
        "currency": {
          "symbol": "BTC",
          "decimals": 8,
          "metadata": {
            "Issuer": "Satoshi"
          }
        },
        "metadata": {}
      },
      "related_transactions": [
        {
          "transaction_identifier": {
            "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
          }
        }
      ],
      "metadata": {}
    }
  ],
  "metadata": {}
}
```

- **/staking-period/slash -** returns slashes for given staking period

Request:

```json
{
  "network_identifier": {
    "blockchain": "bitcoin",
    "network": "mainnet",
    "sub_network_identifier": {
      "network": "shard 1",
      "metadata": {
        "producer": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5"
      }
    }
  },
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  }
}
```

Response:

```json
{
  "staking_period_identifier": {
    "index": 100,
    "name": "epoch"
  },
  "slashes": [
    {
      "slash_index": 0,
      "slash_identifier": {
        "hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
      },
      "type": "offline",
      "staking_period": {
        "index": 100,
        "name": "epoch"
      },
      "account": {
        "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
        "sub_account": {
          "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "metadata": {}
        },
        "metadata": {}
      },
      "amount": {
        "value": "1238089899992",
        "currency": {
          "symbol": "BTC",
          "decimals": 8,
          "metadata": {
            "Issuer": "Satoshi"
          }
        },
        "metadata": {}
      },
      "related_transactions": [
        {
          "transaction_identifier": {
            "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
          }
        }
      ],
      "metadata": {}
    },
    {
      "slash_index": 1,
      "slash_identifier": {
        "hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
      },
      "type": "offline",
      "staking_period": {
        "index": 100,
        "name": "epoch"
      },
      "account": {
        "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
        "sub_account": {
          "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
          "metadata": {}
        },
        "metadata": {}
      },
      "amount": {
        "value": "1238089899992",
        "currency": {
          "symbol": "BTC",
          "decimals": 8,
          "metadata": {
            "Issuer": "Satoshi"
          }
        },
        "metadata": {}
      },
      "related_transactions": [
        {
          "transaction_identifier": {
            "hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
          }
        }
      ],
      "metadata": {}
    }
  ],
  "metadata": {}
}
```
