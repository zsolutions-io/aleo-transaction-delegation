# aleo-transaction-delegation

Install Demox Labs Wasm SDK:

```bash
npm install @demox-labs/aleo-sdk-staging
```

To generate the authorization locally:

In order to generate the authorization & fee_authorization objects, you can use the sdk I linked above:

```js
import * as Aleo from '@demox-labs/aleo-sdk-staging';

// Example values
const privateKey = "APrivateKey1zkpB9CQGb99DPQtAXgS9pPvLzwMK2xcJmY2GeqbEjQYPyZq";
const feeRecord = "{owner:aleo1vxmvr7qw4z3mxe6lm73dvqq0upydlxjf8uy5fyx2slc3yqa3kg8s6jrww7.private,microcredits:1000000u64.private}";
const program = "credits.aleo";
const functionName = "transfer_private";
const inputs = [
  "{owner:aleo1vxmvr7qw4z3mxe6lm73dvqq0upydlxjf8uy5fyx2slc3yqa3kg8s6jrww7.private,microcredits:12300000u64.private}",
  "aleo1yuwhln927wr45ssezn0c2tnhlwp3g32xkmmgvqs4vs7t3rspt58qzmdcjc",
  "50000u64"
];
const feeCredits = 0.5;

const aleoPrivateKey = Aleo.PrivateKey.from_string(privateKey);
const aleoRecord = feeRecord ? Aleo.RecordPlaintext.fromString(feeRecord) : undefined;

const authJson = await Aleo.ProgramManager.authorize_transaction(
  aleoPrivateKey,
  program,
  functionName,
  inputs,
  feeCredits,
  aleoRecord,
  imports
);
const auth = JSON.parse(authJson);
console.log(auth.authorization, auth.fee_authorization);
```

To generate a request:

```sh
curl --location '<https://testnet3.aleorpc.com>' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "generateTransaction",
    "params": {
        "authorization": STRING,
        "program": STRING,
        "fee_authorization": STRING,
        "function": STRING,
        "broadcast": BOOLEAN,
        "imports": Object key program name: STRING to program code: STRING
    }
}'
```

To get the status of that request:

```sh
curl --location '<https://testnet3.aleorpc.com>' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getGeneratedTransaction",
    "params": {
        "request_id": STRING
    }
}'
```

An example request for the free service looks like:

```sh
curl --location '<https://testnet3.aleorpc.com>' \
--header 'Content-Type: application/json' \
--data '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "generateTransaction",
    "params": {
        "authorization": "{\"requests\":[{\"signer\":\"aleo144v3n2afgqlhgtxngqka77twh043lx59kr852j8xee3uu5epnqxsvscuuu\",\"network\":\"3u16\",\"program\":\"credits.aleo\",\"function\":\"transfer_private\",\"input_ids\":[{\"type\":\"record\",\"commitment\":\"1602938028117508648887725822439077642987211842956539214664047077559707728427field\",\"gamma\":\"4667919183028464199179312889590885407543113856156387206202811116885361976334group\",\"serial_number\":\"1204573313122291947656806681880749169403501082070139954335052012554336590921field\",\"tag\":\"2952214804055462495903421528674506665555007612794807152002590056391110857910field\"},{\"type\":\"private\",\"id\":\"993584950168987946042345208065963836705349307355332026930564594834293259595field\"},{\"type\":\"private\",\"id\":\"1080851383919790983541718654671458204914841258755951081961434437624829855333field\"}],\"inputs\":[\"{\\n  owner: aleo144v3n2afgqlhgtxngqka77twh043lx59kr852j8xee3uu5epnqxsvscuuu.private,\\n  microcredits: 14995000u64.private,\\n  _nonce: 7080204549289632170027006311459021299074576212739402094755640075761595981692group.public\\n}\",\"aleo1peg44mdj50mk03erfwapc9u02az33pmwdp3uzltg27upvrz6wu9sjcvx2j\",\"120000u64\"],\"signature\":\"sign16wm9ydmq9rurupnsz67zqnf6kna8e6m0g95dca34vtwen3lnwyqgh3q6a274xek2jdvceaa4es35wapn4d46z4c0t88r92y5mwlfxpqvr2eyy2rwya07z326fmlx63xgfh09a330kkzhrm7k0m8nqf2nzx8urgffudjx6pa8gv5lep7kc5svhca23re5w8wdenfzzlynkulssw6epqr\",\"sk_tag\":\"7784002353752323677183926893954303311022804418232962884687725817791302140668field\",\"tvk\":\"5148747527707616240284845531931460232902742149247033651727446819341658006383field\",\"tcm\":\"3357005845155034833383487769493468686865102325907952104173751946697859681750field\"}],\"transitions\":[{\"id\":\"au184q2t66z0vj7kr7em2h02j4nyttu6rjuxru8lxqdv6kr9fxd2s9qdujp4e\",\"program\":\"credits.aleo\",\"function\":\"transfer_private\",\"inputs\":[{\"type\":\"record\",\"id\":\"1204573313122291947656806681880749169403501082070139954335052012554336590921field\",\"tag\":\"2952214804055462495903421528674506665555007612794807152002590056391110857910field\"},{\"type\":\"private\",\"id\":\"993584950168987946042345208065963836705349307355332026930564594834293259595field\",\"value\":\"ciphertext1qgq2l6fpsa6pqklg0dll0tdwdxesz5r2qaqqk2ut4hpf8ts3kyjtuyptu5t6h9csdflxk8eweeysem4ngt4gne2sjhnz7qrlvygqnz66pc5w2fe3\"},{\"type\":\"private\",\"id\":\"1080851383919790983541718654671458204914841258755951081961434437624829855333field\",\"value\":\"ciphertext1qyqv74lqqnp99hypter4d899a3alkg53fzhdfjz30kywxykk9wae6zgyhf0a3\"}],\"outputs\":[{\"type\":\"record\",\"id\":\"3674557136204851284318964747205379525534461540002277141804604497894566550906field\",\"checksum\":\"8136945614035053995826879782142333119386524801645878297377321389473146167924field\",\"value\":\"record1qyqsq4a5tezeqmnnylram07j85k5ln4rh8jvdyr2se886th503rhqkg3qyxx66trwfhkxun9v35hguerqqpqzqxvrrc2gy77mrs2a9xkjv07yjlvu9fv58ctjc6xv9dljea4yz0gp2xcv8z7xz2xq92kjyvlqt3m42u2x9sdy90q37hx37cwzcl8hm8sv3jx4wf\"},{\"type\":\"record\",\"id\":\"4415389801242724653489115282988670884145964747225173460853383481382885050411field\",\"checksum\":\"7207999054464986944752520035507534514434305444092952639946477159504051057229field\",\"value\":\"record1qyqsqvxwn482taerekt39xr6tux9nrnzkcf6quqe8c5tdk3a2gcegjcpqyxx66trwfhkxun9v35hguerqqpqzqrajdks27cpgtl30wdz8kapttzapp24pkdcqvd0lq2gvc274ps8zynvmza0uu2v8pmxs2u2r9gedv0lh992pn0kkvqlmw2xc7dgt2fs6p92jae\"}],\"tpk\":\"7131099229765411110395871934847779316426348032338935075172500575072806258658group\",\"tcm\":\"3357005845155034833383487769493468686865102325907952104173751946697859681750field\"}]}",
        "program": "program credits.aleo;\n\nmapping committee:\n    key as address.public;\n    value as committee_state.public;\n\nstruct committee_state:\n    microcredits as u64;\n    is_open as boolean;\n\nmapping bonded:\n    key as address.public;\n    value as bond_state.public;\n\nstruct bond_state:\n    validator as address;\n    microcredits as u64;\n\nmapping unbonding:\n    key as address.public;\n    value as unbond_state.public;\n\nstruct unbond_state:\n    microcredits as u64;\n    height as u32;\n\nmapping account:\n    key as address.public;\n    value as u64.public;\n\nrecord credits:\n    owner as address.private;\n    microcredits as u64.private;\n\nfunction bond_public:\n    input r0 as address.public;\n    input r1 as u64.public;\n    gte r1 1000000u64 into r2;\n    assert.eq r2 true ;\n    async bond_public self.caller r0 r1 into r3;\n    output r3 as credits.aleo/bond_public.future;\n\nfinalize bond_public:\n    input r0 as address.public;\n    input r1 as address.public;\n    input r2 as u64.public;\n    is.eq r0 r1 into r3;\n    branch.eq r3 true to bond_validator;\n    branch.eq r3 false to bond_delegator;\n    position bond_validator;\n    cast 0u64 true into r4 as committee_state;\n    get.or_use committee[r0] r4 into r5;\n    assert.eq r5.is_open true ;\n    add r5.microcredits r2 into r6;\n    cast r6 r5.is_open into r7 as committee_state;\n    cast r1 0u64 into r8 as bond_state;\n    get.or_use bonded[r0] r8 into r9;\n    assert.eq r9.validator r1 ;\n    add r9.microcredits r2 into r10;\n    gte r10 1000000000000u64 into r11;\n    assert.eq r11 true ;\n    cast r1 r10 into r12 as bond_state;\n    get account[r0] into r13;\n    sub r13 r2 into r14;\n    set r7 into committee[r0];\n    set r12 into bonded[r0];\n    set r14 into account[r0];\n    branch.eq true true to end;\n    position bond_delegator;\n    contains committee[r0] into r15;\n    assert.eq r15 false ;\n    get committee[r1] into r16;\n    assert.eq r16.is_open true ;\n    add r16.microcredits r2 into r17;\n    cast r17 r16.is_open into r18 as committee_state;\n    cast r1 0u64 into r19 as bond_state;\n    get.or_use bonded[r0] r19 into r20;\n    assert.eq r20.validator r1 ;\n    add r20.microcredits r2 into r21;\n    gte r21 10000000u64 into r22;\n    assert.eq r22 true ;\n    cast r1 r21 into r23 as bond_state;\n    get account[r0] into r24;\n    sub r24 r2 into r25;\n    set r18 into committee[r1];\n    set r23 into bonded[r0];\n    set r25 into account[r0];\n    position end;\n\nfunction unbond_public:\n    input r0 as u64.public;\n    async unbond_public self.caller r0 into r1;\n    output r1 as credits.aleo/unbond_public.future;\n\nfinalize unbond_public:\n    input r0 as address.public;\n    input r1 as u64.public;\n    cast 0u64 0u32 into r2 as unbond_state;\n    get.or_use unbonding[r0] r2 into r3;\n    add block.height 360u32 into r4;\n    contains committee[r0] into r5;\n    branch.eq r5 true to unbond_validator;\n    branch.eq r5 false to unbond_delegator;\n    position unbond_validator;\n    get committee[r0] into r6;\n    sub r6.microcredits r1 into r7;\n    get bonded[r0] into r8;\n    assert.eq r8.validator r0 ;\n    sub r8.microcredits r1 into r9;\n    gte r9 1000000000000u64 into r10;\n    branch.eq r10 true to decrement_validator;\n    branch.eq r10 false to remove_validator;\n    position decrement_validator;\n    cast r7 r6.is_open into r11 as committee_state;\n    set r11 into committee[r0];\n    cast r0 r9 into r12 as bond_state;\n    set r12 into bonded[r0];\n    add r3.microcredits r1 into r13;\n    cast r13 r4 into r14 as unbond_state;\n    set r14 into unbonding[r0];\n    branch.eq true true to end;\n    position remove_validator;\n    assert.eq r6.microcredits r8.microcredits ;\n    remove committee[r0];\n    remove bonded[r0];\n    add r3.microcredits r8.microcredits into r15;\n    cast r15 r4 into r16 as unbond_state;\n    set r16 into unbonding[r0];\n    branch.eq true true to end;\n    position unbond_delegator;\n    get bonded[r0] into r17;\n    sub r17.microcredits r1 into r18;\n    gte r18 10000000u64 into r19;\n    branch.eq r19 true to decrement_delegator;\n    branch.eq r19 false to remove_delegator;\n    position decrement_delegator;\n    get committee[r17.validator] into r20;\n    sub r20.microcredits r1 into r21;\n    cast r21 r20.is_open into r22 as committee_state;\n    set r22 into committee[r17.validator];\n    cast r17.validator r18 into r23 as bond_state;\n    set r23 into bonded[r0];\n    add r3.microcredits r1 into r24;\n    cast r24 r4 into r25 as unbond_state;\n    set r25 into unbonding[r0];\n    branch.eq true true to end;\n    position remove_delegator;\n    get committee[r17.validator] into r26;\n    sub r26.microcredits r17.microcredits into r27;\n    cast r27 r26.is_open into r28 as committee_state;\n    set r28 into committee[r17.validator];\n    remove bonded[r0];\n    add r3.microcredits r17.microcredits into r29;\n    cast r29 r4 into r30 as unbond_state;\n    set r30 into unbonding[r0];\n    position end;\n\nfunction unbond_delegator_as_validator:\n    input r0 as address.public;\n    async unbond_delegator_as_validator self.caller r0 into r1;\n    output r1 as credits.aleo/unbond_delegator_as_validator.future;\n\nfinalize unbond_delegator_as_validator:\n    input r0 as address.public;\n    input r1 as address.public;\n    get committee[r0] into r2;\n    assert.eq r2.is_open false ;\n    contains committee[r1] into r3;\n    assert.eq r3 false ;\n    get bonded[r1] into r4;\n    assert.eq r4.validator r0 ;\n    sub r2.microcredits r4.microcredits into r5;\n    cast r5 r2.is_open into r6 as committee_state;\n    cast 0u64 0u32 into r7 as unbond_state;\n    get.or_use unbonding[r1] r7 into r8;\n    add r8.microcredits r4.microcredits into r9;\n    add block.height 360u32 into r10;\n    cast r9 r10 into r11 as unbond_state;\n    set r6 into committee[r0];\n    remove bonded[r1];\n    set r11 into unbonding[r1];\n\nfunction claim_unbond_public:\n    async claim_unbond_public self.caller into r0;\n    output r0 as credits.aleo/claim_unbond_public.future;\n\nfinalize claim_unbond_public:\n    input r0 as address.public;\n    get unbonding[r0] into r1;\n    gte block.height r1.height into r2;\n    assert.eq r2 true ;\n    get.or_use account[r0] 0u64 into r3;\n    add r1.microcredits r3 into r4;\n    set r4 into account[r0];\n    remove unbonding[r0];\n\nfunction set_validator_state:\n    input r0 as boolean.public;\n    async set_validator_state self.caller r0 into r1;\n    output r1 as credits.aleo/set_validator_state.future;\n\nfinalize set_validator_state:\n    input r0 as address.public;\n    input r1 as boolean.public;\n    get committee[r0] into r2;\n    cast r2.microcredits r1 into r3 as committee_state;\n    set r3 into committee[r0];\n\nfunction transfer_public:\n    input r0 as address.public;\n    input r1 as u64.public;\n    async transfer_public self.caller r0 r1 into r2;\n    output r2 as credits.aleo/transfer_public.future;\n\nfinalize transfer_public:\n    input r0 as address.public;\n    input r1 as address.public;\n    input r2 as u64.public;\n    get account[r0] into r3;\n    sub r3 r2 into r4;\n    set r4 into account[r0];\n    get.or_use account[r1] 0u64 into r5;\n    add r5 r2 into r6;\n    set r6 into account[r1];\n\nfunction transfer_private:\n    input r0 as credits.record;\n    input r1 as address.private;\n    input r2 as u64.private;\n    sub r0.microcredits r2 into r3;\n    cast r1 r2 into r4 as credits.record;\n    cast r0.owner r3 into r5 as credits.record;\n    output r4 as credits.record;\n    output r5 as credits.record;\n\nfunction transfer_private_to_public:\n    input r0 as credits.record;\n    input r1 as address.public;\n    input r2 as u64.public;\n    sub r0.microcredits r2 into r3;\n    cast r0.owner r3 into r4 as credits.record;\n    async transfer_private_to_public r1 r2 into r5;\n    output r4 as credits.record;\n    output r5 as credits.aleo/transfer_private_to_public.future;\n\nfinalize transfer_private_to_public:\n    input r0 as address.public;\n    input r1 as u64.public;\n    get.or_use account[r0] 0u64 into r2;\n    add r1 r2 into r3;\n    set r3 into account[r0];\n\nfunction transfer_public_to_private:\n    input r0 as address.private;\n    input r1 as u64.public;\n    cast r0 r1 into r2 as credits.record;\n    async transfer_public_to_private self.caller r1 into r3;\n    output r2 as credits.record;\n    output r3 as credits.aleo/transfer_public_to_private.future;\n\nfinalize transfer_public_to_private:\n    input r0 as address.public;\n    input r1 as u64.public;\n    get account[r0] into r2;\n    sub r2 r1 into r3;\n    set r3 into account[r0];\n\nfunction join:\n    input r0 as credits.record;\n    input r1 as credits.record;\n    add r0.microcredits r1.microcredits into r2;\n    cast r0.owner r2 into r3 as credits.record;\n    output r3 as credits.record;\n\nfunction split:\n    input r0 as credits.record;\n    input r1 as u64.private;\n    sub r0.microcredits r1 into r2;\n    sub r2 10000u64 into r3;\n    cast r0.owner r1 into r4 as credits.record;\n    cast r0.owner r3 into r5 as credits.record;\n    output r4 as credits.record;\n    output r5 as credits.record;\n\nfunction fee_private:\n    input r0 as credits.record;\n    input r1 as u64.public;\n    input r2 as u64.public;\n    input r3 as field.public;\n    assert.neq r1 0u64 ;\n    assert.neq r3 0field ;\n    add r1 r2 into r4;\n    sub r0.microcredits r4 into r5;\n    cast r0.owner r5 into r6 as credits.record;\n    output r6 as credits.record;\n\nfunction fee_public:\n    input r0 as u64.public;\n    input r1 as u64.public;\n    input r2 as field.public;\n    assert.neq r0 0u64 ;\n    assert.neq r2 0field ;\n    add r0 r1 into r3;\n    async fee_public self.caller r3 into r4;\n    output r4 as credits.aleo/fee_public.future;\n\nfinalize fee_public:\n    input r0 as address.public;\n    input r1 as u64.public;\n    get account[r0] into r2;\n    sub r2 r1 into r3;\n    set r3 into account[r0];\n",
        "fee_authorization": "{\"requests\":[{\"signer\":\"aleo144v3n2afgqlhgtxngqka77twh043lx59kr852j8xee3uu5epnqxsvscuuu\",\"network\":\"3u16\",\"program\":\"credits.aleo\",\"function\":\"fee_private\",\"input_ids\":[{\"type\":\"record\",\"commitment\":\"5104138025844046682125029769268961732790653201745691236072185304532073129745field\",\"gamma\":\"3065816018843036320491749361082297117372069372332409058571009981166452685216group\",\"serial_number\":\"255343359693612624233300430807905985136226977076000052710605385098287917431field\",\"tag\":\"3259569267816101798212433653906644451132958306372886842973613398782075587094field\"},{\"type\":\"public\",\"id\":\"3985446965538914507365197251182366083733271306623983184395659298332163641119field\"},{\"type\":\"public\",\"id\":\"3439926811808262614510368405166016896723836126523672196384772695922240492232field\"},{\"type\":\"public\",\"id\":\"1991011490870180682923825644546692495790841674242368726567858931460458506293field\"}],\"inputs\":[\"{\\n  owner: aleo144v3n2afgqlhgtxngqka77twh043lx59kr852j8xee3uu5epnqxsvscuuu.private,\\n  microcredits: 14877000u64.private,\\n  _nonce: 6934582725274837449845074684764788551364611329947573216400057872167330851572group.public\\n}\",\"5000u64\",\"0u64\",\"3696704041063303086977290234539084139754534817144423838186825051048381680833field\"],\"signature\":\"sign10f3mcthnzxcteg3ahmu0c0j22p32ft6rfq2xexrwqtkxv8nj4qpw4mjnqdrms54tmmmyejgqef40jdzay4rmxewfwnkwnwm7s0lrqqqvr2eyy2rwya07z326fmlx63xgfh09a330kkzhrm7k0m8nqf2nzx8urgffudjx6pa8gv5lep7kc5svhca23re5w8wdenfzzlynkulsslqc49m\",\"sk_tag\":\"7784002353752323677183926893954303311022804418232962884687725817791302140668field\",\"tvk\":\"5009669084824638517588333355614082542235430014851717813449974773565125728292field\",\"tcm\":\"5716419792671060419989403033893141614947611018634205844707230743786220864399field\"}],\"transitions\":[{\"id\":\"au1v9zjdk7c5ep4kltfqx6e3eneaxun035lehde2g879wnyk7m5pcpsacnvsv\",\"program\":\"credits.aleo\",\"function\":\"fee_private\",\"inputs\":[{\"type\":\"record\",\"id\":\"255343359693612624233300430807905985136226977076000052710605385098287917431field\",\"tag\":\"3259569267816101798212433653906644451132958306372886842973613398782075587094field\"},{\"type\":\"public\",\"id\":\"3985446965538914507365197251182366083733271306623983184395659298332163641119field\",\"value\":\"5000u64\"},{\"type\":\"public\",\"id\":\"3439926811808262614510368405166016896723836126523672196384772695922240492232field\",\"value\":\"0u64\"},{\"type\":\"public\",\"id\":\"1991011490870180682923825644546692495790841674242368726567858931460458506293field\",\"value\":\"3696704041063303086977290234539084139754534817144423838186825051048381680833field\"}],\"outputs\":[{\"type\":\"record\",\"id\":\"4471401842909578645543205696843782283523387842845965942306135300960730297237field\",\"checksum\":\"6293477049172639502383006093964282558101479019977778202367007728142211699498field\",\"value\":\"record1qyqspn4296fj96ugsz4qdjsuxq9egu7aurmtkagc9u5rp9rcgf68rnssqyxx66trwfhkxun9v35hguerqqpqzqrr0w4vy7lcswcgjvzp8tzl6d0vzvf4948y3gp4sh9mhy6yfratqrzufzh2h73fsg0qkzmwmaqlwwcq5mggwn35hzk04y5qcw63aj9skyp64ln\"}],\"tpk\":\"492821730828829198126857704655592227711225543316072445148159147188006437860group\",\"tcm\":\"5716419792671060419989403033893141614947611018634205844707230743786220864399field\"}]}",
        "function": "transfer_private",
        "broadcast": true,
        "imports": {}
    }
}'
```
