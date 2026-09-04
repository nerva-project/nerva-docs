# Nerva Multisig Guide
Multisignature, usually shortened to multisig, is a wallet scheme where several people share control of one address and a configurable number of them must cooperate to spend. A 2-of-3 multisig wallet, for example, has three participants and needs any two of them to sign a transaction. It is the standard tool for shared funds: escrow between two parties with an arbiter, treasury held by several maintainers, or simply storing a backup of your own funds across several machines so that losing one does not lose the coins.

This page explains the current status of multisig in Nerva, which is not the usual how-to, and what you should do if you are holding funds in a multisig wallet today.

<hr>

# Current status: disabled for new wallets

Nerva inherited its multisig implementation from the upstream code base it forked from. That implementation predates the key exchange rework upstream performed after the 2022 multisig disclosures, and upstream still labels even its fixed version experimental. Nerva's copy is the older, unaudited variant.

Because of this, creating new multisig wallets has been disabled. Attempting the enrollment commands produces a direct error:

    Error creating multisig: Multisig is disabled: the implementation is legacy
    and unaudited. Existing multisig wallets can spend until fork 14.

The same applies to the offline `gen_multisig` helper that used to generate whole multisig wallet sets in one go, since it builds on the same disabled machinery. The commands still appear in the wallet's help, but they cannot complete.

# If you have an existing multisig wallet

Wallets that were already multisig before the feature was disabled keep opening, syncing and receiving funds normally. Spending from them keeps working until hard fork 14. The activation height in the source is currently 4,500,000, but that is a placeholder held deliberately far out so that builds from master do not fork early; the real height is set once the validation gates pass and is expected to land earlier than that. Treat it as a moving deadline rather than a date you can plan around, and watch the announcement channels.

From that point on, multisig wallets will no longer be able to spend at all. Recent wallet versions print a warning at startup to make this hard to miss:

    WARNING: multisig is a disabled legacy feature. From fork 14 this wallet
    cannot spend; move any funds before the fork.

Because the date can move earlier, do not wait for it. If you hold funds in a multisig wallet, the practical advice is simple: **move them to a regular wallet now**. Gather the required signers, spend the balance to a normal wallet you control, and treat the multisig address as spent afterwards. Receiving continues to work after the fork, but coins sent to a multisig address after it would be stuck, so decommission the address properly and stop giving it out now.

# Why it was disabled

The reasoning is worth writing down because it is the same reasoning that would bring the feature back.

Multisig in this code family works by splitting a spend key across participants and having them exchange partial keys in a setup phase. The way that exchange was originally constructed turned out to have subtle problems, which came to light in 2022 and prompted upstream to redesign the protocol. Nerva never ported that redesign, so its multisig remains the old construction, unreviewed against those known issues. Keeping enrollment open would mean telling users "shared custody of your funds is protected by a scheme with known disclosed weaknesses", which is not something a small project should say. Disabling new wallets while leaving existing ones time to move is the conservative path.

# Outlook

Bringing multisig back means porting and reviewing the reworked key exchange, and no timeline has been announced for that work. Until it happens, there is no supported way to create a new multisig wallet in Nerva, and any tool that claims to do it for you should be treated with suspicion.

If shared control of funds is a hard requirement for you today, the workarounds are imperfect by nature: split custody across separate regular wallets with agreed withdrawal procedures, or wait for the feature to return. Watch the usual announcement channels for hard fork plans, since a multisig rework would necessarily ship through one.
