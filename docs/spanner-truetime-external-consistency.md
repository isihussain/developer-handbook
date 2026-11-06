# Google Spanner: TrueTime & External Consistency

Google Spanner achieves global strict serializability (external consistency) using synchronized GPS and atomic clock hardware.

---

## 1. TrueTime API

Returns an uncertainty window:
$$	ext{TT.now}() = [t_{	ext{earliest}}, t_{	ext{latest}}] \quad 	ext{where } t_{	ext{latest}} - t_{	ext{earliest}} = 2\epsilon$$

### Commit Wait Rule
A transaction assigned timestamp $s$ must pause and cannot return commit success to the client until:
$$	ext{TT.now}().	ext{earliest} > s$$
Guarantees that any subsequent transaction globally will receive a commit timestamp $s' > s$.
