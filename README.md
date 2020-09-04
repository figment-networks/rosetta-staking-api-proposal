# Rosetta Staking API proposal

In JSON file, attributes in bold are objects taken from official Rosetta Data API spec.

## 1. Identifiers

- **Staking period identifier - I**dentifies the staking period. The smallest period is one block, but for instance in Polkadot we also have sessions and eras

```json
{
    "staking_period_identifier": {
        "index": 100,
        "name": "epoch"
    }
}
```

- **Operator identifier -** One of the staking participants. This is the operator of the network on whom stakers can stake on. In most blockchains it is called validator but in some cases it is called something else ie. in Livepeer they are called transcoders

```json
{
    "operator_identifier": {
        "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
    }
}
```

- **Staker identifier -** One of the staking participants. . This is the entity who stakes coins on operators. In different blockchains it is called differently. Sometime it is referred to as delegator and sometimes as nominator.

```json
{
  "staker_identifier": {
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
		"name": "epoch",
	},
	"start_height": 1,
	"end_height": 100,
	"start_time": 12435456,
	"end_time": 15435456,
	"metadata": {}
}
```

- **Staking participant** - Entity participating in staking. Can be a validator

```json
{
	"staking_participant_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"role": "validator", // staker | transcoder etc.
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
		"email": "validator@figment.com"
		"website": "http://figment.com"
	},
	"metadata": {}
}
```

- **Operator** - Network operator with required main account and optional identity

```json
{
	"operator_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"role": "validator", //can also be transcoder in case of Livepeer
	**"account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
        "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
        "metadata": {}
    },
    "metadata": {}
	},**
	"identity": {
		"name": "Figment",
		"logo": "https://fiment.com/logo.png",
		"email": "validator@figment.com"
		"website": "http://figment.com"
	},
	"metadata": {}
}
```

- **Identity -** Identity information indicating who the staking participant really is.

```json
{
	"name": "Figment",
	"logo": "https://fiment.com/logo.png",
	"email": "validator@figment.com",
	"website": "http://figment.com",
	"metadata": {}
}
```

- **Staker** - identifies staker (delegator/nominator)

```json
{
	"staker_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"role": "staker",
	**"account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
        "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
        "metadata": {}
    },
    "metadata": {}
	},**
	"metadata": {}
}
```

- **Stake** - identifies `staker_identifier` who stakes `amount` on `operator_identifier` for a `staking_period_identifier`

```json
{
	"staking_period": {
		"index": 100,
		"name": "epoch",
	},
	"operator": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"staker": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	**"amount": {
      "value": "1238089899992",
      "currency": {
          "symbol": "BTC",
          "decimals": 8,
          "metadata": {
              "Issuer": "Satoshi"
          }
      },
      "metadata": {}
  }**
}
```

- **Unstake** - identifies `staker_identifier` who stakes `amount` on `operator_identifier` for a `staking_period_identifier`

```json
{
	"staking_period": {
		"index": 100,
		"name": "epoch",
	},
	"operator": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"staker": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	**"amount": {
      "value": "1238089899992",
      "currency": {
          "symbol": "BTC",
          "decimals": 8,
          "metadata": {
              "Issuer": "Satoshi"
          }
      },
      "metadata": {}
  },**
	"metadata": {}
}
```

- **Reward** - identifies reward being paid to `account_identifier` with the `amount` at the end of `staking_period_identifier`

```json
{
	"reward_identifier": {
		"hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"type": "staking_reward",
	"staking_period": {
		"index": 100,
		"name": "epoch",
	},
	**"account": {
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
  },**
	"related_transactions": [
		**{
			"transaction_identifier": {
				"hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
			}
		}
	],**
	"metadata": {}
}
```

- **Slash** - identifies `account_identifier` which is being slashed by the `amount` at the end of `staking_period_identifier`

```json
{
	"slash_identifier": {
		"hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"type": "offline",
	"staking_period": {
		"index": 100,
		"name": "epoch",
	},
	**"account": {
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
  },**
	"related_transactions": [
		**{
			"transaction_identifier": {
				"hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
			}
		}
	],**
	"metadata": {}
}
```

- **Staking activity** - every activity that is related to staking. 4 main types are:
    - staked
    - unstaked
    - rewarded
    - slashed

```json
{
	"staking_activity_identifier": {
		"hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"staking_period": {
		"index": 100,
		"name": "epoch",
	},
	"type": "staked",
	"participants": [
		{
			"participant_index": 0,
			"operator_identifier": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
			}
			**"**staking_activity_role**"**: "target",
			"metadata": {}
		},
		{
			"participant_index": 1,
			"staker_identifier": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61"
			}
			**"**staking_activity_role**"**: "origin",
			"metadata": {}
		}
	],
	"balance_changes": [
		{
			**"account_identifier": {
			    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			    "sub_account": {
			        "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
			        "metadata": {}
			    },
			    "metadata": {}
			},
			"**staking_activity_role": "target",
			"metadata": {}
		}
	],
	"related_transactions": [
		**{
			"transaction_identifier": {
				"hash": "0x2f23fd8cca835af21f3ac375bac601f97ead75f2e79143bdf71fe2c4be043e8f"
			}
		}
	],**
	"metadata": {}
}
```

## 3. Endpoints

- **/staking-period -** returns information about staking period ****

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
		"name": "epoch",
	},
}
```

Response:

```json
{
	"staking_period_identifier": {
		"index": 100,
		"name": "epoch",
	},
	"start_height": 1,
	"end_time": 100,
	"start_timestamp": 1582272577,
	"end_timestamp": 1582272577,
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
	"operator_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	}
}
```

Response:

```json
{
	"operator_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"role": "validator",
	**"account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
        "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
        "metadata": {}
    },
    "metadata": {}
	},**
	"identity": {
		"identity_identifier": {
			"name": "Figment"
		},
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
	"staker_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	}
}
```

Response:

```json
{
	"staker_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	**"account": {
    "address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
    "sub_account": {
        "address": "0x6b175474e89094c44da98b954eedeac495271d0f",
        "metadata": {}
    },
    "metadata": {}
	},**
	"identity": {},
	"metadata": {}
}
```

- **/staking-period/validator** - returns validator information for given staking period

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
	"validator_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"staking_period_identifier": {
		"index": 100,
		"name": "epoch",
	},
}
```

Response:

```json
{
	"validator_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"staking_period": {
		"index": 100,
		"name": "epoch",
	},
	"status": "online",
	"commission": 10000,
	"stakes": [
		{
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			"validator": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"staker": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			**"amount": {
		      "value": "1238089899992",
		      "currency": {
		          "symbol": "BTC",
		          "decimals": 8,
		          "metadata": {
		              "Issuer": "Satoshi"
		          }
		      },
		      "metadata": {}
		  },**
		},
		{
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			"validator": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"staker": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			**"amount": {
		      "value": "1238089899992",
		      "currency": {
		          "symbol": "BTC",
		          "decimals": 8,
		          "metadata": {
		              "Issuer": "Satoshi"
		          }
		      },
		      "metadata": {}
		  },**
		}
	],
	"unstakes": [
		{
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			"validator": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"staker": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			**"amount": {
		      "value": "1238089899992",
		      "currency": {
		          "symbol": "BTC",
		          "decimals": 8,
		          "metadata": {
		              "Issuer": "Satoshi"
		          }
		      },
		      "metadata": {}
		  },**
		}
	],
	"metadata": {}
}
```

- **/staking-period/validator-pool -** returns information about validators in the validator pool for given staking period

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
		"name": "epoch",
	},
}
```

Response:

```json
{
	"staking_period": {
		"index": 100,
		"name": "epoch",
	},
	validators: [{
		"validator_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
		},
		"validator_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
		},
		"validator_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
		},
		"validator_identifier": {
			"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
		}
	}],
	"total_stake": 100000,
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
	"staker_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
}
```

Response:

```json
{
	"staking_period_identifier": {
		"index": 100,
		"name": "epoch"
	},
	"staker_identifier": {
		"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
	},
	"stakes": [
		{
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			"validator": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"staker": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			**"amount": {
		      "value": "1238089899992",
		      "currency": {
		          "symbol": "BTC",
		          "decimals": 8,
		          "metadata": {
		              "Issuer": "Satoshi"
		          }
		      },
		      "metadata": {}
		  },**
		},
		{
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			"validator": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"staker": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			**"amount": {
		      "value": "1238089899992",
		      "currency": {
		          "symbol": "BTC",
		          "decimals": 8,
		          "metadata": {
		              "Issuer": "Satoshi"
		          }
		      },
		      "metadata": {}
		  },**
		}
	],
	"unstakes": [
		{
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			"validator": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"staker": {
				"address": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			**"amount": {
		      "value": "1238089899992",
		      "currency": {
		          "symbol": "BTC",
		          "decimals": 8,
		          "metadata": {
		              "Issuer": "Satoshi"
		          }
		      },
		      "metadata": {}
		  },**
		}
	],
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
	},
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
			"reward_identifier": {
				"hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"type": "staking_reward",
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			**"account": {
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
		  },**
			"related_transactions"**: [],**
			"metadata": {}
		},
		{
			"reward_identifier": {
				"hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"type": "staking_reward",
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			**"account": {
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
		  },**
			"related_transactions"**: [],**
			"metadata": {}
		}
	],
	"metadata": {}
}
```

- **/staking-period/slash -**

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
			"slash_identifier": {
				"hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"type": "offline",
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			**"account": {
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
		  },**
			"metadata": {}
		},
		{
			"slash_identifier": {
				"hash": "0x3a065000ab4183c6bf581dc1e55a605455fc6d61",
			},
			"type": "offline",
			"staking_period": {
				"index": 100,
				"name": "epoch",
			},
			**"account": {
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
		  },**
			"metadata": {}
		}
	],
	"metadata": {}
}
```