# AVALANCHE HYPERSDK

This is a demonstration of how to create and interact with a custom subnet using avalanche HyperSDK

## Description


This program simply deploys a custom subnet in which a user can mint, create, and even fill out orders for individuals that are in need of a certain token after minting
it


## Getting Started

### Executing program

To run this program, you need to download go from https://go.dev/doc/install and note that the version should be 1.21.4 and above for it to work, it should be installed
on linux or a windows subsystem for linux. Next, clone this repository https://github.com/Metacrafters/tokenvm to the directory of your choice.

```javascript

// Copyright (C) 2023, Ava Labs, Inc. All rights reserved.
// See the file LICENSE for licensing terms.

package registry

import (
	"github.com/ava-labs/avalanchego/utils/wrappers"
	"github.com/ava-labs/avalanchego/vms/platformvm/warp"
	"github.com/ava-labs/hypersdk/chain"
	"github.com/ava-labs/hypersdk/codec"

	"tokenvm/actions"
	"tokenvm/auth"
	"tokenvm/consts"
)

// Setup types
func init() {
	consts.ActionRegistry = codec.NewTypeParser[chain.Action, *warp.Message]()
	consts.AuthRegistry = codec.NewTypeParser[chain.Auth, *warp.Message]()

	errs := &wrappers.Errs{}
	errs.Add(
		// When registering new actions, ALWAYS make sure to append at the end.
		consts.ActionRegistry.Register(&actions.Transfer{}, actions.UnmarshalTransfer, false),

		// TODO: register action: actions.CreateAsset
		// TODO: register action: actions.MintAsset

		consts.ActionRegistry.Register(&actions.CreateAsset{}, actions.UnmarshalCreateAsset, false),
		consts.ActionRegistry.Register(&actions.MintAsset{}, actions.UnmarshalMintAsset, false),

		
		consts.ActionRegistry.Register(&actions.BurnAsset{}, actions.UnmarshalBurnAsset, false),
		consts.ActionRegistry.Register(&actions.ModifyAsset{}, actions.UnmarshalModifyAsset, false),

		consts.ActionRegistry.Register(&actions.CreateOrder{}, actions.UnmarshalCreateOrder, false),
		consts.ActionRegistry.Register(&actions.FillOrder{}, actions.UnmarshalFillOrder, false),
		consts.ActionRegistry.Register(&actions.CloseOrder{}, actions.UnmarshalCloseOrder, false),

		consts.ActionRegistry.Register(&actions.ImportAsset{}, actions.UnmarshalImportAsset, true),
		consts.ActionRegistry.Register(&actions.ExportAsset{}, actions.UnmarshalExportAsset, false),

		// When registering new auth, ALWAYS make sure to append at the end.
		consts.AuthRegistry.Register(&auth.ED25519{}, auth.UnmarshalED25519, false),
	)
	if errs.Errored() {
		panic(errs.Err)
	}
}


```

paste this code on consts.go

```javascript

// Copyright (C) 2023, Ava Labs, Inc. All rights reserved.
// See the file LICENSE for licensing terms.

package consts

import (
	"github.com/ava-labs/avalanchego/ids"
	"github.com/ava-labs/avalanchego/vms/platformvm/warp"
	"github.com/ava-labs/hypersdk/chain"
	"github.com/ava-labs/hypersdk/codec"
	"github.com/ava-labs/hypersdk/consts"
)

const (
	// TODO: choose a human-readable part for your hyperchain
	HRP = "Hello world"
	// TODO: choose a name for your hyperchain
	Name = "Hello world token"
	// TODO: choose a token symbol
	Symbol = "HWT"
)

var ID ids.ID

func init() {
	b := make([]byte, consts.IDLen)
	copy(b, []byte(Name))
	vmID, err := ids.ToID(b)
	if err != nil {
		panic(err)
	}
	ID = vmID
}

// Instantiate registry here so it can be imported by any package. We set these
// values in [controller/registry].
var (
	ActionRegistry *codec.TypeParser[chain.Action, *warp.Message, bool]
	AuthRegistry   *codec.TypeParser[chain.Auth, *warp.Message, bool]
)


```


First, go to https://metacrafterc-scmstarter-m0r8y82q8c8.ws-us105.gitpod.io

To get the code working, follow these steps:

1. Clone the initial repository
2. Inside your project folder, run go mod tidy to normalize all the dependencies
3. Configure your project constants
4. Go to consts/consts.go and add the missing parts.
5. Register the Create_Asset and Mint_Assest actions in registry/registry.go
6. Run your VM locally
7. Make sure Go is on your path, defined on your terminal, if not you can do so by running export PATH=$PATH:$(go env GOPATH)/bin
8. If this path doesn’t work, you can also try export PATH=$PATH:/usr/local/go/bin
9. Run MODE="run-single" ./scripts/run.sh
10. Run ./scripts/build.sh
11. If you get a permissions denied error, try running these scripts with the bash command (i.e.bash ./scripts/run.sh)
12. Load the demo private key included on the project ./build/token-cli key import demo.pk and ./build/token-cli chain import-anr
13. Interact with your own HyperChain!
14. Use the demos included in the README file or located at the repo in step 1
15. To close your Local Avalanche Network run killall avalanche-network-runner

## Authors

Metacrafter Kyle  
"# Avalanche-Subnets" 
